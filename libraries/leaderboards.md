---
description: Add scores and display a bounded leaderboard with TopBoardLib, including reset behavior.
---


# Show a leaderboard

`TopBoardLib` is available in the runtime. Use it for a small ranking such as a game's top ten players. It stores a score per user and a bounded display board.

## Add a score

Call this only after your bot has verified the event that earns points:

{% code title="Add a score · Example 1" overflow="wrap" %}
```javascript
TopBoardLib.addScore({
  boardName: "weekly-game",
  value: 5,
  maxCount: 10
});
```
{% endcode %}

`value` is the score increment, not the final score. Supply a nonzero number. `maxCount` defaults to 10 and must be between 2 and 25. `boardName` defaults to `default`.

To credit someone else, provide `user` inside the options object:

{% code title="Add a score · Example 2" overflow="wrap" %}
```javascript
TopBoardLib.addScore({
  boardName: "weekly-game",
  value: 5,
  user: { id: 123, telegramid: 987654321, first_name: "Example" }
});
```
{% endcode %}

Replace the example IDs with a verified user's internal BB ID and Telegram ID. Passing that user object directly to `addScore` is not the correct contract.

## Display the board

In `/top`:

{% code title="Display the board · Example 3" overflow="wrap" %}
```javascript
const board = TopBoardLib.getBoard("weekly-game");
const lines = board.map(function (entry, index) {
  return (index + 1) + ". " + entry.tgId + ": " + entry.value;
});
Api.sendMessage({
  text: lines.length ? lines.join("\n") : "No scores yet."
});
```
{% endcode %}

Entries contain `id`, `tgId`, available name fields and `value`. Extra keys supplied in `fields` are merged directly into a new entry; they are not nested under `entry.fields`. Do not use custom fields named `id`, `tgId` or `value`, because those can overwrite the entry's identity or score. Custom fields on an existing entry are not refreshed by this implementation.

## Start a new season

`resetBoard("weekly-game")` clears the displayed board, **not the per-user score properties**. A user who earns points afterward can reappear with their earlier cumulative score.

For a fresh season, choose a new board name such as `game-2026-10` and use it consistently in both the scoring and display commands. `getUserPropName(boardName)` exposes the corresponding score property name if you need to inspect it.

## Limits

The board retains only its top entries. It is not a complete ordered directory of every player, and updates use ordinary property reads and writes. A [List](../bjs/lists.md) can store a paginated collection, subject to its current ordering limits. Strict simultaneous-update guarantees require a storage design that provides atomic updates; switching to a List alone does not provide those guarantees.

See [Referrals](referrals.md) for a ranking driven by invitations rather than your own game events.
