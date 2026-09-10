---
description: Check channel membership with MembershipChecker, configure callbacks, and avoid using cached membership as a
  fresh verification.
---


# Check channel membership

Install `MembershipChecker` to ask Telegram whether a user has joined your configured chats or channels. The check completes through callback commands. Reading cached membership and starting a new check are different operations.

Before checking someone else with `getChatMember`, make the bot an administrator in each target chat; that is the condition under which Telegram guarantees this method for other users. [Telegram reference](https://core.telegram.org/bots/api#getchatmember).

{% stepper %}

{% step %}

### Configure the panel

1. Run `Libs.MembershipChecker.setup()` from an owner-only setup command.
2. Open **Admin Panel → Membership checker options** in the mobile app.
3. Enter target chats, separated by commas, for example `@example_channel`.
4. Choose the automatic checking delay in minutes.
5. Set `onNeedJoining` to `/join-required`, `onJoining` to `/membership-updated`, `onAllJoining` to `/all-joined`, and `onError` to `/membership-error`.
6. Save the panel. Do not run setup again casually: it writes the panel definition and defaults.

{% endstep %}

{% step %}

### Start with a manual check

In `/check`:

{% code title="Start with a manual check · Example 1" overflow="wrap" %}
```javascript
if (!user || chat.chat_type !== "private") { return; }
Libs.MembershipChecker.check();
Bot.sendMessage("Checking your membership…");
```
{% endcode %}

In `/join-required`:

{% code title="Start with a manual check · Example 2" overflow="wrap" %}
```javascript
if (!options || !options.chat_id) { return; }
Bot.sendMessage("Please join " + options.chat_id + ", then send /check.");
```
{% endcode %}

In `/membership-updated`:

{% code title="Start with a manual check · Example 3" overflow="wrap" %}
```javascript
Bot.sendMessage("Membership updated.");
```
{% endcode %}

In `/all-joined`:

{% code title="Start with a manual check · Example 4" overflow="wrap" %}
```javascript
Bot.sendMessage("You have joined all required channels.");
```
{% endcode %}

In `/membership-error`:

{% code title="Start with a manual check · Example 5" overflow="wrap" %}
```javascript
Bot.sendMessage("Could not check membership. Please try again later.");
```
{% endcode %}

Inspect the detailed error privately in your test bot when diagnosing configuration. Do not send raw provider responses or internal state to every user.

{% endstep %}

{% endstepper %}

## Read the cached decision

`Libs.MembershipChecker.isMember()` returns the stored all-chats decision. Pass one chat ID to inspect that target only. `getChats()` returns the configured comma-separated string; `getNotJoinedChats()` returns a comma-separated list still missing from the cached state.

`check()` schedules new checks and throttles calls within two seconds for a user. It does not immediately return the fresh Telegram result. `handle()` uses the configured delay and can be called for suitable incoming commands after the first check.

The checked version's automatic `handle()` does not initialize a user with no previous check timestamp. Keep an explicit `/check` entry; do not rely on `handle()` alone for the first visit.

## Callback meanings and limitations

- `onNeedJoining`: a target is not joined; this can run once per target.
- `onNeedAllJoining`: all targets are recorded as not joined in that check.
- `onJoining`: a newly joined target.
- `onAllJoining`: newly reaches all joined; in this version configure `onJoining` too, because that branch precedes the all-joined callback.
- `onStillJoined`: still joined after an explicit check.
- `onError`: the API check failed.

This version recognizes `member`, `administrator` and `creator`. Telegram's restricted-member state needs an explicit policy; do not assume it is accepted. Custom callback data propagation also differs from the older `bb_option` examples, so use your own recorded state when the action requires context.

For a sensitive action, perform a fresh verified check in that action's flow instead of relying on old membership. Increasing the checking delay reduces repeated work but makes cached state older. For ordinary repeated button presses, see [Cooldowns](cooldown.md).
