---
description: Configure message dictionaries and let each bot user choose a language with the Lang library.
---

# Translate bot messages with Lang

Install `Lang` to store message dictionaries and a language choice for each bot user. It does not translate text automatically. Supply the translations you want the bot to send.

## Configure two languages

Run this once from an owner-only setup command:

```javascript
Libs.Lang.setup("en", {
  greeting: "Hello!",
  account: { empty: "No saved details yet." }
});
Libs.Lang.setup("es", {
  greeting: "¡Hola!",
  account: { empty: "Todavía no hay datos guardados." }
});
Libs.Lang.default.setLang("en");
```

The first configured language becomes the default unless you explicitly change it. Re-running setup replaces the dictionary stored under that language name, so include all its keys.

In `/hello`:

```javascript
Bot.sendMessage(Libs.Lang.t("greeting"));
Bot.sendMessage(Libs.Lang.t("account.empty"));
```

Keys such as `account.empty` are paths in your own dictionary. Keep them fixed in your code; do not pass arbitrary user text as a translation expression.

## Let a user choose

In `/english`:

```javascript
Libs.Lang.user.setLang("en");
Bot.sendMessage(Libs.Lang.t("greeting"));
```

In `/spanish`, use `Libs.Lang.user.setLang("es")`. Configure the dictionary before letting users select it. A selected language with no configured dictionary raises an error.

## Reference

| Call | Result |
| --- | --- |
| `Libs.Lang.user.getCurLang()` | User's selected language, or default |
| `Libs.Lang.default.getCurLang()` | Bot's default language |
| `Libs.Lang.get("en")` | Full dictionary for that language |
| `Libs.Lang.t("key", "en")` | Translation in an explicitly selected language |
| `Libs.Lang.getCommandByAlias(text, language)` | Matching command name from an `aliases` dictionary |

`t()` falls back to the default language for a missing or falsy value. Use nonempty message strings; an empty string, `false` or numeric zero is not a reliable translation value for this fallback behavior.

An aliases dictionary can map visible labels to commands:

```javascript
Libs.Lang.setup("en", {
  greeting: "Hello!",
  aliases: { "Help, Support": "/help" }
});
```

`getCommandByAlias` performs a case insensitive lookup. It returns a name; your code decides whether to run it. Translation keys and BJS method names stay unchanged across languages.

If you need whole reply layouts with buttons and placeholders, use [SmartBot templates](smart-bot.md). Its language storage is separate from `Libs.Lang`.
