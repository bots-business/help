---
description: Create referral links, track a user's first entry, and read referral counts with RefLib.
---


# Track referral links

`RefLib` creates Telegram start links and records who invited a new bot user. It is included in the runtime. The compatibility name is `Libs.ReferralLib`.

Referral tracking identifies an invitation; it does not prove that the invited person paid, completed a task, or is a distinct human. Decide those conditions separately before giving rewards.

{% stepper %}

{% step %}

### Create the invitation link

In `/invite`, use your bot's Telegram username **without `@`**:

{% code title="Create the invitation link · Example 1" overflow="wrap" %}
```javascript
const link = RefLib.getLink("YOUR_BOT_USERNAME", "invite");
Api.sendMessage({ text: "Invite a friend:\n" + link });
```
{% endcode %}

`getLink` also records the inviting user's details for later attribution. The payload contains the user's internal BB ID; do not replace it with `user.telegramid`. If you choose a prefix such as `invite`, use the same prefix when tracking.

{% endstep %}

{% step %}

### Track the first start

Put this in `/start` before your ordinary welcome message:

{% code title="Track the first start · Example 2" overflow="wrap" %}
```javascript
RefLib.track({
  linkPrefix: "invite",
  onAttracted: function (referrer) {
    Bot.sendMessage("Invitation recorded. Welcome!");
  },
  onTouchOwnLink: function () {
    Bot.sendMessage("This is your own invitation link.");
  },
  onAlreadyAttracted: function () {
    Bot.sendMessage("Welcome back.");
  }
});
```
{% endcode %}

The callbacks are JavaScript functions, not command names. `onAttracted` receives the recorded referrer object. Its `id` is an internal BB ID and `telegramid` is a Telegram user ID.

Tracking marks an ordinary first start without a referral as an existing user too. A later invitation therefore does not overwrite the first-entry decision. Do not call `track()` from an unrelated setup command and expect that user still to count as new.

{% endstep %}

{% step %}

### Read the result

In `/referrals`:

{% code title="Read the result · Example 3" overflow="wrap" %}
```javascript
Bot.sendMessage("People you invited: " + RefLib.getRefCount());
const referrer = RefLib.getAttractedBy();
if (referrer) {
  Bot.sendMessage("You were invited by BB user " + referrer.id);
}
```
{% endcode %}

`getRefCount(otherBbUserId)` reads another referrer's count. Avoid calling it for several different users in one execution: these counts share a property name and are affected by the current [cross-user property lookup limitation](../bjs/user-properties.md#reading-several-users-in-one-execution).

`getRefList()` returns the current user's referral List; `getRefList(otherBbUserId)` selects another referrer's List. By default, `getTopList().get()` returns the bounded, score-sorted TopBoard entries reshaped as `{ user, value }`; it is not a paginated List of every referrer. Legacy `useList` mode has different storage and inherits the current [List ordering limits](../bjs/lists.md). Keep the default for a small leaderboard, and do not switch an existing bot's storage mode without migrating its counts.

{% endstep %}

{% endstepper %}

<details>
<summary>Older referral examples</summary>

## Older referral examples

Use the direct methods for new code:

| Older call | Direct call |
| --- | --- |
| `RefLib.currentUser.getRefLink(botName, prefix)` | `RefLib.getLink(botName, prefix)` |
| `RefLib.currentUser.track(options)` | `RefLib.track(options)` |
| `RefLib.currentUser.refList.get()` | `RefLib.getRefList()` |
| `RefLib.currentUser.attractedByUser()` | `RefLib.getAttractedBy()` |

The same replacements apply when older code uses `Libs.ReferralLib` as the prefix. These are compatibility wrappers around the direct methods; changing the spelling alone does not reset referrals or change the storage mode.

</details>

## Reward a verified action

First confirm the action in the command that owns it. Then find `RefLib.getAttractedBy()` and grant the reward once using your own recorded action ID. A ResourcesLib recipient is addressed with the referrer's **`telegramid`**, even though RefLib counts use **`id`**.

Never treat a `/start` payload, a user-supplied amount, or a payment return URL as proof that the action happened. A simple property check is useful for ordinary repeated commands, but it does not by itself make simultaneous financial callbacks atomic.

## Troubleshooting

- Test with two accounts and an invited account that has not started this bot before.
- Generate the referrer's link with `getLink` before using it.
- Check the username and matching prefix.
- `clearRef()` resets the current user's attribution flags for debugging. It does not undo all referral counts or rewards. `clearRefList()` is not implemented; do not use it as a reset operation.

For general start parameters, see [Deep links](../guides/deep-links.md).
