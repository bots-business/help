---
description: Build a working English and Spanish SmartBot menu with reusable titles, types, references, and a per-user language
  switch.
---


# Build a two-language SmartBot menu

This example keeps menu structure in one place and supplies English and Spanish text separately. Each user can choose a language. `SmartBot` is built into the BJS runtime; no Store library is needed.

Use a fresh test bot. Create `/setup`, `/smart-menu`, `/smart-help`, `/smart-about`, `/smart-language`, and `/smart-lang`. Keep **Answer** and **Keyboard** empty, **Wait for answer** off, and Auto Retry empty for all six commands. Do not add a global `@` or `@@` handler for this example.

{% stepper %}

{% step %}

### Save both language dictionaries

Follow [trusted administrator setup](../bjs/security.md#restrict-a-command-to-a-trusted-telegram-user). In `/setup`, replace `YOUR_TELEGRAM_USER_ID` with your own Telegram user ID, keeping the quotes:

{% code title="/setup" overflow="wrap" %}
```javascript
// Command: /setup
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }

// The structure and command names stay the same in every language.
const commands = {
  "/smart-menu": {
    text: "#/texts/menu", inline_buttons: "#/keyboards/main", parse_mode: "HTML"
  },
  "/smart-help": {
    text: "#/texts/help", inline_buttons: "#/keyboards/back", parse_mode: "HTML"
  },
  "/smart-about": {
    text: "#/texts/about", inline_buttons: "#/keyboards/back", parse_mode: "HTML"
  },
  "/smart-language": {
    text: "#/texts/language", inline_buttons: "#/keyboards/languages", parse_mode: "HTML"
  }
};
const keyboards = {
  main: [
    [{ text: "{helpButton}", command: "/smart-help" },
     { text: "{aboutButton}", command: "/smart-about" }],
    [{ text: "{languageButton}", command: "/smart-language" }]
  ],
  back: [[{ text: "{menuButton}", command: "/smart-menu" }]],
  languages: [
    [{ text: "English", command: "/smart-lang en" },
     { text: "Español", command: "/smart-lang es" }],
    [{ text: "{menuButton}", command: "/smart-menu" }]
  ]
};
const smart = new SmartBot();
saveLanguage("en", {
  appTitle: "Tea shop", helpButton: "Help", aboutButton: "About",
  languageButton: "Language", menuButton: "Menu"
}, {
  menu: "{appTitle}\nChoose an option.",
  help: "Choose a button below to return to {appTitle}.",
  about: "{appTitle} is a demonstration menu.",
  language: "Choose a language."
});
saveLanguage("es", {
  appTitle: "Tienda de té", helpButton: "Ayuda", aboutButton: "Acerca de",
  languageButton: "Idioma", menuButton: "Menú"
}, {
  menu: "{appTitle}\nElige una opción.",
  help: "Pulsa el botón para volver a {appTitle}.",
  about: "{appTitle} es un menú de demostración.",
  language: "Elige un idioma."
});
Bot.sendMessage("SmartBot languages saved. Send /smart-menu.");

function saveLanguage(language, titles, texts) {
  smart.setupLng(language, {
    commands: commands,
    titles: titles,
    types: { keyboards: keyboards, texts: texts }
  });
}
```
{% endcode %}

Send `/setup` from your configured Telegram account. The first saved language is the default on this fresh bot, so a user without a language selection starts in English. Re-run `/setup` after changing the dictionaries; editing the command alone does not update the stored templates.

{% endstep %}

{% step %}

### Render the four screens

Paste this same complete BJS into **each** of `/smart-menu`, `/smart-help`, `/smart-about`, and `/smart-language`:

{% code title="Render the four screens · Example 2" overflow="wrap" %}
```javascript
// Commands: /smart-menu, /smart-help, /smart-about, /smart-language
if (!user || !chat) { return; }
const smart = new SmartBot();
smart.handle();
```
{% endcode %}

Each command selects its matching entry in `commands`. A title such as `{appTitle}` comes from the selected language's `titles`. A whole field such as `"#/keyboards/back"` resolves a reusable value under `types`: both Help and About use that same keyboard.

A `#/...` reference must be the **whole field value** here. Writing `"Current menu: #/texts/menu"` does not embed another template inside that sentence. Use `{placeholders}` for text substitution. Keep command names, dictionary keys, and placeholder names unchanged in translations; translate their text values.

SmartBot inline buttons use nested rows with `text` and `command` (or `url`). The runtime turns `command` into Telegram callback data. This is different from `Bot.sendInlineKeyboard`, which uses `title` for a button label.

{% endstep %}

{% step %}

### Change the current user's language

Put this in `/smart-lang`:

{% code title="/smart-lang" overflow="wrap" %}
```javascript
// Command: /smart-lang
if (!user || !chat) { return; }
const language = (params || "").trim();
if (language !== "en" && language !== "es") {
  Bot.sendMessage("Use /smart-lang en or /smart-lang es.");
  return;
}
const smart = new SmartBot();
smart.setUserLang(language);
Bot.runCommand("/smart-menu");
```
{% endcode %}

The allowlist accepts only configured languages. `setUserLang` saves the current user's choice; the following command creates a new SmartBot instance and loads that language. Calling `handle()` on the old instance would still use the dictionary it already loaded.

{% endstep %}

{% step %}

### Test the whole menu

1. Send `/setup`, then `/smart-menu`. Expect `Tea shop` and `Choose an option.` with **Help**, **About**, and **Language** buttons.
2. Tap **Help**, then **Menu**. Check that the reused back button returns to the menu.
3. Tap **Language**, then **Español**. Expect `Tienda de té` and `Elige una opción.`, with **Ayuda**, **Acerca de**, and **Idioma**.
4. Send `/smart-help` and `/smart-about` to check both translated screens and their **Menú** button.
5. Send `/smart-menu` in a later message; Spanish should remain selected for this user. Another user should still see English until they choose a language.
6. Use **Idioma → English** to switch back. `/smart-lang fr` should display usage instructions and leave the current choice unchanged.

All text in this exercise is controlled by the developer. HTML templates do not escape arbitrary user input: escape it before inserting it, and avoid passing secrets as template parameters. For the rest of the API, see [SmartBot, SmartAmountDialog, and SmartTasker](../libraries/smart-bot.md).

{% endstep %}

{% endstepper %}
