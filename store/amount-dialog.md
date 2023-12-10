# Amount Dialog

## Overview

SmartAmountDialog is a tool designed to help process and validate numerical inputs from users, especially when it involves monetary amounts.

This tool is handy for checking conditions such as minimum and maximum amounts, integer-only inputs, and ensuring the amount doesn't exceed the user's current balance.

### Key Features

* **Minimum and Maximum Values**: Set boundaries for the input amount.
* **Integer-Only Values**: Checks if the input number is an integer.
* **Customizable Error Messages**: User-friendly messages for each type of error.

## How It Works

First, you need to create an instance of `SmartAmountDialog` with necessary parameters. Here’s an example of how to do this:

```javascript
let smartAmountDialog = new SmartAmountDialog({
  min: 10,                // Minimum acceptable amount
  max: 100,               // Maximum acceptable amount
  curValue: 50,           // Current amount (e.g., user's balance)
  onlyInteger: true,      // Requires only integer numbers
  skipZero: false,        // Don't skip zero values
  dialogErrors: {         // Error messages
    invalid: "Not a number",
    zero: "Your balance is zero",
    notEnough: "Not enough funds",
    small: "Amount too small",
    big: "Amount too large",
    notInteger: false // accept float on false and integer only on true
  }
});
```

## Usage

To use `SmartAmountDialog` to check an input amount, you call the `accept` method and pass the string value to it. If the method returns `true`, it means the input value passed all checks.

Otherwise, you can retrieve the error message from `errMsg`.

Example:

```javascript
let userInput = "30"; // Imagine this is the value entered by the user
let isValid = smartAmountDialog.accept(userInput);

if (isValid) {
  Bot.sendMessage("This amount is valid: " + smartAmountDialog.amount)
} else {
  Bot.sendMessage("Error: " + smartAmountDialog.errMsg);
}
```

## Using with SmartBot

For example, we already have:

* smartBot object
* command with "acceptAmount" name. It have checked **Wait For Answer** option.

### Template

We can use SmartBot with Command Template:

```javascript
// Template for command "acceptAmount"
    acceptAmount:{
      // any other fieilds here
      /// ...
      // we use "dialogErrors" section for error messages
      dialogErrors: {
        // if user enter not a number
        invalid: "❌ *Invalid amount.*\n \"{_amount}\" - not valid." +
          "\n\nPlease enter valid amount for withdraw request (max: {_curValue}).",

        // if user have zero balance
        zero: "Your balance is zero. \n\nPlease complete any task before.",

        // if user enter amount more than balance
        notEnough: "❌ *Invalid amount.*\n \"{_amount}\" - not enough balance." +
          "\n\nPlease enter valid amount for withdraw request (max: {_curValue}).",

        // if user enter less then min amount
        small: "❌ *Invalid amount.*\n \"{_amount}\" - too small." +
          "\n\nPlease enter valid amount for withdraw request (min: {_min}, max: {_curValue}).",

        // if user enter too big then max amount
        big: "❌ *Invalid amount.*\n \"{_amount}\" - too big." +
          "\n\nPlease enter valid amount for withdraw request (max: {_max}).",

        // if user enter not integer amount
        notInteger: "❌ *Invalid amount.*\n \"{_amount}\" - not integer."
      }
    },
```

**All fields:**&#x20;

| Field      | Description                         |
| ---------- | ----------------------------------- |
| \_amount   | amount for validation               |
| \_min      | minimal value                       |
| \_max      | maximal value                       |
| \_curValue | current value - amount must be less |

### Initialization

```javascript
let smartAmountDialog = new SmartAmountDialog({
  // options like befire:
  min: 10,                // Minimum acceptable amount
  max: 100,               // Maximum acceptable amount
  curValue: 50,           // Current amount (e.g., user's balance)
  onlyInteger: true,      // Requires only integer numbers
  skipZero: false,        // Don't skip zero values
  // we need to pass smartbot now:
  smart_bot: smartBot,
  // and we pass errors from Template:
  dialogErrors: smartBot.curCommand.dialogErrors
});
```

then we can use checking as before
