---
description: Learn JavaScript variables, types, conditions, arrays, objects, loops, and functions with small BJS examples you can run in your Telegram bot.
---

# JavaScript basics for BJS beginners

JavaScript is the language you use to write BJS commands. Its variables, conditions, loops, and functions let your bot calculate a result and choose what to say. Bots.Business adds tools such as `Bot.sendMessage` for sending that result to Telegram.

You do not need previous programming experience. Work through the examples in order, then combine them in a small `/quote` command at the end.

## Set up a practice command

First, [create a bot that replies](../start/first-bot.md). In the mobile app, open its **Commands** and create `/practice`:

- Leave **Answer**, **Keyboard**, and **Auto retry time in seconds** empty.
- Turn **Wait for answer** off and leave **Allowed only for group** empty.
- Put the example in the BJS code editor, tap **Save**, and send `/practice` in a private Telegram conversation with the running bot.

```javascript
Bot.sendMessage("Hello from BJS!");
```

The reply is `Hello from BJS!`. `Bot.sendMessage` is a function supplied by Bots.Business. The parentheses contain what you pass to it; the quotes mark text. The semicolon ends the statement.

For each example below, **replace the previous code completely**. Do not paste all examples into one command: several reuse the same variable names. You need no extra libraries. The final example has its own command name, `/quote`.

## Variables: give a value a name

A variable gives a value a name so you can use it later in the code.

```javascript
let points = 2;
points = points + 3;
const label = "Points: ";
Bot.sendMessage(label + points);
```

The reply is `Points: 5`.

- `let points = 2` creates a variable with the value `2`.
- `points = points + 3` reads the old value, adds `3`, and stores the result.
- `const label` creates a variable that you cannot assign a different value to later. Use `const` when you do not need reassignment; use `let` when you do.
- `+` adds two numbers, or joins text when a string is involved.

You will also see `var` in older BJS examples. It declares a variable too, but its scope rules differ; start with `const` and `let` in your own code.

Names are case-sensitive: `points` and `Points` are different variables. Choose names such as `itemPrice` or `totalPoints`. Do not reuse the built-in BJS names `user`, `chat`, `Bot`, `User`, `Api`, or `params` for your own variables.

Text after `//` is a comment for the reader; JavaScript does not execute it.

## Types: text, numbers, and true or false

| Type | Example | Use |
| --- | --- | --- |
| String | `"Hello"` | Text, including text received in chat |
| Number | `4` or `2.5` | Counts and calculations |
| Boolean | `true` or `false` | A yes/no value |
| Null | `null` | An explicit empty value |
| Undefined | `undefined` | A value that has not been supplied or assigned |

`"4"` is text; `4` is a number. `false` is a Boolean; `"false"` is text.

```javascript
const textNumber = "2";
Bot.sendMessage(textNumber + 1);
Bot.sendMessage(String(Number(textNumber) + 1));
```

The bot sends `21`, then `3`. `Number(...)` converts text to a number, and `String(...)` converts a value to text. Text such as `"apple"` cannot become an ordinary number: `Number("apple")` produces `NaN`, meaning “not a number.” Validate chat input before calculating with it.

Use matching straight quotes, either `"text"` or `'text'`. Inside a string, `\n` starts a new line. Curly quotation marks copied from formatted text can cause a syntax error.

## Calculations

```javascript
const itemPrice = 4;
const quantity = 3;
const total = itemPrice * quantity;
Bot.sendMessage("Total: " + total + " points");
```

The reply is `Total: 12 points`. Use `+` to add, `-` to subtract, `*` to multiply, and `/` to divide. Parentheses control order: `(2 + 3) * 4` is `20`.

These examples use whole points. JavaScript decimal arithmetic can have rounding differences; do not treat this beginner exercise as a payment implementation.

## Conditions: choose what happens

`if` runs a block only when its condition is true. `else` handles the other case.

```javascript
const points = 12;
const required = 10;

if (points >= required) {
  Bot.sendMessage("You have enough points.");
} else {
  Bot.sendMessage("You need more points.");
}
```

The reply is `You have enough points.` Change `points` to `5` and run it again to reach the other branch. Curly braces `{ ... }` group the statements in a block. Use `else if` between the first branch and `else` when you need another condition.

| Operator | Meaning |
| --- | --- |
| `===` | Equal value and type: `2 === 2` is true; `2 === "2"` is false |
| `!==` | Different value or type |
| `>` or `<` | Greater than or less than |
| `>=` or `<=` | Greater/less than, or equal |
| `&&` | Both conditions must be true |
| `||` | At least one condition must be true |
| `!` | Reverse a value's true/false interpretation |

Use `===` for equality checks. A single `=` assigns a value; it does not compare values. For example, `quantity >= 1 && quantity <= 5` checks both ends of a range.

Some values also count as false in a condition: `0`, `""`, `null`, `undefined`, and `NaN`. Non-empty text, including `"false"`, counts as true. Prefer an explicit comparison when you need a particular value.

## Arrays and objects: group related values

An **array** is an ordered list. An **object** holds named fields.

```javascript
const topics = ["Commands", "Buttons", "BJS"];
const product = { name: "Notebook", price: 4 };

Bot.sendMessage(topics[0]);
Bot.sendMessage(product.name + ": " + product.price);
```

The replies are `Commands` and `Notebook: 4`.

- `topics[0]` reads the first item. Array indexes start at zero, so the third item is `topics[2]`.
- `topics.length` is the number of items: `3` here. `topics.push("Properties")` would append another item.
- `product.name` reads a named field. `product.price = 5` would change that field.
- `const` stops you assigning a different array or object to the variable; it does not freeze the contents.

`user.first_name` follows the same field-reading idea. BJS supplies that object for executions with a current user; see [context and variables](context.md) before using it in other kinds of command.

## Loops: repeat a small amount of work

A `for` loop can visit every item in an array. Build one reply and send it after the loop:

```javascript
const topics = ["Commands", "Buttons", "BJS"];
let reply = "Topics:\n";

for (let i = 0; i < topics.length; i++) {
  reply = reply + (i + 1) + ". " + topics[i] + "\n";
}

Bot.sendMessage(reply);
```

The reply is:

```text
Topics:
1. Commands
2. Buttons
3. BJS
```

The three parts of `for` mean: start `i` at zero; keep going while `i` is less than the length; add one to `i` after each pass. `i++` is a short way to increment it. The `i + 1` in the reply makes the visible numbering start at one.

A `while` loop repeats while a condition stays true:

```javascript
let count = 1;
let reply = "";

while (count <= 3) {
  reply = reply + count + " ";
  count++;
}

Bot.sendMessage(reply.trim());
```

The reply is `1 2 3`. `trim()` removes whitespace from the start and end. Without `count++`, this loop would never reach its stopping condition.

Keep loops bounded. A loop cannot wait for the user's next Telegram message, and repeatedly sending messages in a loop can flood a chat. Use [Wait for answer](../app/collect-input.md) to collect the next reply, [scheduling](background.md) for later work, or [broadcasts](broadcasts.md) for many recipients.

## Functions: name a reusable calculation

A function groups code you want to call. Its parameters receive the values you pass in; `return` gives a result back to the caller.

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

const total = calculateTotal(4, 3);
Bot.sendMessage("Total: " + total + " points");
```

The reply is `Total: 12 points`. Inside the function, `price` is `4` and `quantity` is `3`. A function definition alone does not run its body: `calculateTotal(4, 3)` is the call. A function without a returned value produces `undefined`.

Function parameters are local to that function. `let` and `const` variables declared inside an `if`, loop, or function block are only available within their scope. Declare a variable before using it, and declare it outside a block if later code needs it.

A JavaScript function in this command is different from another Bots.Business command. `Bot.runCommand("/other")` does not give you a JavaScript return value from `/other`; use the documented [command and callback flow](README.md#how-execution-works).

## Put it together: a `/quote` command

Create `/quote` with the same empty metadata fields as `/practice`, paste the complete example, and tap **Save**. The user supplies a quantity after the command name, such as `/quote 3`.

```javascript
const input = String(params || "").trim();
if (input === "") {
  Bot.sendMessage("Send /quote followed by a quantity from 1 to 5.");
  return;
}

const quantity = Number(input);
if (!Number.isInteger(quantity) || quantity < 1 || quantity > 5) {
  Bot.sendMessage("Choose a whole quantity from 1 to 5.");
  return;
}

const product = { name: "Notebook", price: 4 };

function calculateTotal(price, count) {
  return price * count;
}

const total = calculateTotal(product.price, quantity);
const lines = [
  "Item: " + product.name,
  "Quantity: " + quantity,
  "Total: " + total + " points"
];
Bot.sendMessage(lines.join("\n"));
```

In BJS, `params` contains the text after the command name. `params || ""` uses an empty string when no value is supplied; here `||` selects a fallback value. `Number.isInteger(...)` checks that conversion produced a whole number, and the remaining conditions check its range. `lines.join("\n")` joins the array into one message with line breaks.

Each early `return` stops this command's BJS before it builds the quote. This example keeps **Answer** and **Keyboard** empty; BJS `return` does not suppress those separate metadata fields.

| Send in Telegram | Expected result |
| --- | --- |
| `/quote 3` | Item: Notebook; Quantity: 3; Total: 12 points, on separate lines |
| `/quote 1` or `/quote 5` | Totals of 4 or 20 points |
| `/quote` | Instructions to supply a quantity |
| `/quote apple`, `/quote 2.5`, `/quote 0`, or `/quote 6` | A request for a whole quantity from 1 to 5 |

This command only calculates a quote. It does not save an order, deduct points, or create a payment.

## What survives the next message?

A local variable belongs to one execution. Running `let points = 2` again starts at `2` again; it does not remember the previous run. Other commands do not inherit these local variables or function definitions.

When you need a saved name, balance, or setting, continue with [user and bot properties](user-properties.md). That guide explains what belongs to one user and what is shared by the bot.

## Practice and troubleshoot

Try changing the product name and price, adding an item to the topics array, or making `/quote` accept quantities up to `10`. Update its validation and help messages together, then test a valid value, an invalid value, and missing input.

If something fails, check spelling, matching quotes and braces, `=` versus `===`, and whether a value is text or a number. Use **Check** in the editor for syntax and the bot's **Errors** for execution problems. Save changes before testing in Telegram.

An example from a browser tutorial may use `document` or `alert`; a Node.js tutorial may use `require`. These are not BJS message tools. Use `Bot.sendMessage` to see a result in this lesson and follow [BJS execution and limits](README.md) when moving on to callbacks and integrations.
