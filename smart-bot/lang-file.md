# Lang File

## Introduction

Language files are a crucial part of developing a multilingual bot using `SmartBot`. They allow you to separate the textual content from the logic of your bot, making it easier to manage and update translations.&#x20;

This guide will walk you through the process of creating and setting up a language file for `SmartBot`.

## Structure

A language file in `SmartBot` is a JSON object that contains keys and values. The keys represent command names, message types, or other identifiers, while the values hold the actual text or translations.

#### Basic Structure

```json
{
  "commands": {
    "/start": {
      "text": "Welcome to our bot!"
    },
    "/help": {
      "text": "Here's how you can use the bot..."
    }
  },
  "types": {
    // General types and templates
  },
  "titles": {
    // General titles or labels used in the commands
    // will be added to params
    // can be accessed via params and via {name} in template
  }
  // Additional sections as needed
}
```

### Rules for Writing Language Files

1. **Do Not Translate Keys**: Never translate the keys in the language file; only translate the values.
2. **Preserve Masks in Curly Braces**: Any text within `{}` should not be translated. These are placeholders for dynamic content.
3. **Avoid Changing the Structure**: Maintain the JSON structure as defined in your bot's code.

### Adding Commands and Responses

Each command your bot can execute should have a corresponding entry in the language file. For example:

```json
"commands": {
  "/start": {
    "text": "Welcome, {username}! Ready to start your journey?"
  },
  "/balance": {
    "text": "Your current balance is {balance} coins."
  }
}
```

### Command Structure in Language Files

In `SmartBot`, the commands section of the language file plays a crucial role. Each command the bot can execute should have a corresponding entry in this section. The structure allows for dynamic content insertion and supports various functionalities like text responses, keyboard layouts, and media handling.

#### Basic Template for Commands

Each command entry can include the following properties:

```json
"commandName": {
  "text": "Response text with {variable}",
  "parse_mode": "HTML",  // default is "Markdown"
  "chat_id": "Chat ID"   // or @channelname",
  "alias": "Command alias",
  "aliases": "Back, Cancel",
  "keyboard": "Button1, Button2",
  "inline_buttons": [
    { "text": "Button text", "command": "/command" },
    { "text": "Button text", "url": "https://link.to" }
  ],
  "alert": "Alert text for inline button",
  "alert_top": "Top alert text",
  "photo": "https://link.to/image.png",
  
  // Run another command
  // you can pass run option
  // "run": { command: "/start", params: { any: "param" } }
  
  // Editing:
  // this command can be act as edit command
  // for text or/and keyboard.
  // Default is: false
  // "edit": false,
  // message id for editing
  // "message_id": "{message_id}",
}
```

#### Key Properties Explained

* **text**: The bot's response text. You can embed variables like `{username}` that will be dynamically replaced.
* **parse\_mode**: Defines how the message text should be parsed and formatted. Defaults to "Markdown".
* **chat\_id**: Specifies where the message should be sent. It can be a user ID, a group ID, or a channel username. By default - it is current chat.
* **alias** and **aliases**: Shortcuts or alternative names for commands. Useful for multi-language support or creating intuitive command names. Examples: "Back, Cancel". Alias support spaces. For example: "Go to back" - it is alias: it is not "Go" with params
* **keyboard** and **inline\_buttons**: Define custom keyboards or inline button layouts for interactive responses.
* **alert** and **alert\_top**: Special properties for displaying alerts when inline buttons are pressed.
* **photo**: Directs the bot to send an image. The URL should point to the image file.
* **run**: you can pass "run" for run sub command like `"run": { command: "/start", params: { any: "param" } }`
* **edit**: A boolean flag indicating whether the bot should edit its previous message instead of sending a new one.
* **message\_id**: message id for editing. Please note: SmartBot passed current message id on inline button pressing automatically&#x20;

#### Example: Simple Test Command

Here's a basic example of a test command setup in the language file:

```json
"/test": {
  "text": "Current language file version: #/langVer"
}
```

#### Using Types and References

You can also use predefined types or templates defined in the `types` section of your language file, as shown in the example above with `#/langVer`.

<pre class="language-json"><code class="lang-json">types: {
  langVer: "Lang file version: 1.0.0",
  // you can structure blocks as you wish - it is just a JS object
  // for example "keyboards", "buttons", "alerts", "screens", "groups" and etc
  keyboards: {
    // we can use it as "#/keyboards/joinInlineKeyboard"
    // "#/" - it is key for types
    // only one button here:
    joinInlineKeyboard: [[
      { text: "Check join to {Joining:notJoinedCount} " + 
          "from {Joining:channelsCount} channel(s)",
       command: "checkJoin"  // it is link for command
    }]]
<strong>  }
</strong><strong>}
</strong></code></pre>

then you can use types in commands:

```json
"/test": {
  // just simnple text with type reference:
  // we used general type "langVer" from "types" section
  text: "#/langVer"
},
```

and as keyboard:

```json
"/start": {
  text: "Hello.\nIt is BB Demo Task Bot." +
    "\n\nYou can earn {currency} for completing tasks.\n\n " +
    "*You need to join:* \n {Joining:allChannels} \n\n",

  // we used general type "joinInlineKeyboard" from "types" section
  inline_buttons: "#/keyboards/joinInlineKeyboard"
}
```

## Setting Up the Language File in SmartBot

To support multiple languages, create separate language files for each language, and then use `SmartBot`'s `setupLng` method to load the appropriate file based on user preferences.

Once your language file is ready, you can set it up in `SmartBot` like this - command name "lng-en":

```javascript
const LANG_EN = {
  // Your English translations...
  "commands": {
    // ...
  },
  "types": {
    // ...
  },
  "titles": {
    // ...
  }
};
smartBot.setupLng("en", LANG_EN);
```

another command - 'lng-fr':

```javascript
const LANG_FR = {
  // Your French translations...
};
smartBot.setupLng("fr", LANG_FR);
```



Setup command - /setup:

```javascript
const languages = [
  // the first language item is default!
  //   it is used if user language is not found
  //   and it is English by default!
  {
    // English
    "name": "English",
    "code": "en",
    "flag": "🇺🇸"
  },
  // add anoter languages here
  // you need also add command lng-CODE e.g. lng-fr
  // use command "lng-en" as template
  // {
  //   // French
  //   "name": "Français",
  //   "code": "fr",
  //   "flag": "🇫🇷"
  // },
  // and etc
];

let cmdName;
for(let i in languages){
  cmdName = "lng-" + languages[i].code;
  Bot.run({ command: cmdName })
}
```

Then run /setup command in the bot.

{% hint style="warning" %}
if you change Lang file you need to rerun `/setup` again
{% endhint %}
