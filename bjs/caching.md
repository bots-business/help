---
description: Cache stable BJS command actions with Bot.setCache or User.setCache, invalidate changed content, and avoid sharing private or state-changing responses.
---

# Caching commands

Command caching reuses a command's saved actions for a limited time. It can reduce repeated BJS work for a stable response. It is not a general key-value cache and does not make slow external side effects disappear.

## Cache a shared answer

Create `/hours`:

```javascript
var hours = Bot.getProp("opening_hours", "Monday to Friday, 09:00–17:00");
Bot.sendMessage(hours, { parse_mode: null });
Bot.setCache(300);
```

The command's actions may be reused for five minutes. When changing the underlying value, clear that command's cache:

```javascript
// Use in your authorized settings-save flow.
Bot.setProp("opening_hours", "Monday to Saturday, 09:00–17:00");
Bot.clearCache("/hours");
```

The first command is suitable only when its response is identical for every recipient and its other actions are safe to replay.

## Methods

| Method | Scope |
| --- | --- |
| `Bot.setCache(seconds)` | Cache current-command actions for the bot |
| `User.setCache(seconds)` | Cache current-command actions for the current user |
| `Bot.clearCache(command_name)` | Remove this bot's active shared cache for the named command |
| `User.clearCache(command_name)` | Remove the current user's active cache for the named command |

Call `setCache` inside the command being cached. `clearCache` takes the command's saved name, such as `/hours`, not a property name.

## A per-user answer

```javascript
// Command: /profile-summary
if (!user) { return; }
var nickname = User.getProp("nickname", user.first_name || "Reader");
Bot.sendMessage("Hello, " + nickname + ".", { parse_mode: null });
User.setCache(60);
```

Clear `/profile-summary` with `User.clearCache("/profile-summary")` when that user updates their nickname. Always require a current user for user-cache operations; otherwise the backend has no user ID to scope the entry.

## Choose a safe cache boundary

The cache stores actions associated with a command and optional user. It does not create a separate cache entry for every `params` or `options` value. A command that accepts `/price tea` and `/price coffee` should not use one shared cached answer unless those inputs deliberately produce the same output.

Do not cache:

- Payments, property increments, installations, account recovery, or other actions that must happen once.
- Personalized text with `Bot.setCache`.
- Current authorization checks, a changing permission state, or a one-time token.
- A large HTTP/API workflow on the assumption that caching its actions means its remote result is stored as plain content.

Instead, separate stable text from state-changing work. Cache the stable command and call it from a small workflow where appropriate. Because cache entries replay actions, a repeated request may still perform an action present in the cache.

## Troubleshooting

If users see stale content, check its lifetime and invalidation command. If two users see the same name, replace the shared cache with a user-scoped design and clear the old shared entry. If changing parameters does not change the answer, the cache boundary is too broad.

Caching does not fix unbounded loops, recursive command chains, or incorrect API calls. Start with [BJS errors](errors.md) and the relevant [Bot](bot.md), [HTTP](http.md), or [property](user-properties.md) contract.
