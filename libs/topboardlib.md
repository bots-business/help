# TopBoardLib

`TopBoardLib` is a lib designed for managing a leader board in a chatbot. It provides functionality for adding, updating, retrieving, and resetting user scores, as well as managing multiple leader boards.

## Methods

### addScore(params)

Adds or updates a user's score on the leader board.

**Parameters:**

`params`: An object containing:

* `value` (Number): The score value to be added.
* `boardName` (String, optional): The name of the leader board, default is "default".
* `maxCount` (Number, optional): The maximum number of entries on the board, default is 10, max is 25
* `fields` (Object, optional): Additional fields for the user's entry.

{% hint style="success" %}
You can use "fields" for extra any values. This fields can be used later by you.
{% endhint %}

### getBoard(boardName)

Retrieves a leader board by its name.

**Parameters:**

* `boardName` (String, optional): The name of the leader board, default is "default".

**Returns:**

* An array of entries on the leader board.
* each record it is:&#x20;

```javascript
{
        // user data:
        id: user.id,
        tgId: user.telegramid,
        first_name: user.first_name,
        last_name: user.last_name,
        username: user.username,
        language_code: user.language_code,
        // current value
        value: curValue
        // your passed fields:
        fields: { }
 }
```

### resetBoard(boardName)

Resets the leader board, removing all entries.

**Parameters:**

* `boardName` (String, optional): The name of the leader board, default is "default".

###

## Usage Examples

#### in /gameOver command:

```javascript
// we add +5 to Score
let userScore = User.getProperty("score") || 0;
userScore = userScore + 5;

// Adding points to a user
TopBoardLib.addScore({
   value: 5
   // boardName: "Game" // you can pass boar name
});

```

#### in /top command:

```javascript
// Retrieving the leaderboard
let leaderboard = TopBoardLib.getBoard();

let leaderboardText = "The leaderboard is currently empty.";

// Check if the leaderboard is not empty
if (leaderboard.length > 0) {
  // Create a string to represent the leaderboard
  leaderboardText = "🏆 Leaderboard 🏆\n\n";
  leaderboard.forEach((entry, index) => {
    leaderboardText += `${index + 1}. ${entry.tgId}: ${entry.value} points\n`;
  });
}

// Send the leaderboard
Api.sendMessage({ text: leaderboardText });
```



{% hint style="info" %}
You also can pass another user data to TopBoardLib.addScore method
{% endhint %}

```javascript
// another user:
let another_user = { id: ID, telegramid: tgId, first_name: "Smith" }
another_user.value = 10;

// update leaders board
// another user score will be +10:
TopBoardLib.addScore(another_user);
```

## Other methods

`getUserPropName(boardName)` - this method return prop name for storing user's score. You can use it to prevent duplicate information. For example, if you have a user balance, then you can store it with this property name
