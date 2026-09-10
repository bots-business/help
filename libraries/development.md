---
description: Write a reusable BJS library, expose functions with publish, and import it from a bot repository.
---

# Create a BJS library

Create a library when several commands need the same behavior. Your library is a JavaScript file under `libs/` in a [bot repository](../integrations/repository-format.md). The filename determines the `Libs` name when imported.

## Export two functions

Create `libs/Greetings.js`:

```javascript
function sayHello(name) {
  Bot.sendMessage("Hello, " + name + "!");
}

function sayGoodbye(name) {
  Bot.sendMessage("Goodbye, " + name + ".");
}

publish({
  hello: sayHello,
  goodbye: sayGoodbye
});
```

In a command:

```javascript
Libs.Greetings.hello("Alex");
```

The key passed to `publish` is the public method name. `sayHello` is private here; `Libs.Greetings.sayHello()` is not exported.

## Import and verify

1. Put the library beside valid `bot.json` and `commands/` files.
2. Import into a disposable test bot using [Git import](../integrations/git.md). Git import replaces the destination bot's commands and installed library list.
3. Run the command that calls `Libs.Greetings.hello`.
4. Confirm the reply, then check the bot's library list and exported source.

Use a valid identifier as the library filename, such as `Greetings.js`. Avoid punctuation, spaces, and names reserved for core compatibility libraries. The importer creates library records from `libs/*.js`; it does not recursively import nested library folders.

## Receive an HTTP callback inside a library

The library can register its own command handler with `on(name, function)`:

```javascript
function loadExample() {
  HTTP.get({
    url: "https://example.com",
    success: "Greetings_loaded",
    error: "Greetings_failed"
  });
}
function loaded() {
  Bot.sendMessage("Received " + content.length + " characters.");
}
function failed() {
  Bot.sendMessage("The example page could not be loaded.");
}
on("Greetings_loaded", loaded);
on("Greetings_failed", failed);
publish({ loadExample });
```

Here `publish({ loadExample })` is ordinary JavaScript shorthand for `publish({ loadExample: loadExample })`; the exported name is unchanged. Keep the explicit mapping in `hello: sayHello` when you want a different public name.

Call `Libs.Greetings.loadExample()`. Callback handlers execute in a later command context; use persisted state or explicitly passed data where your workflow needs it. Prefix internal handler names to avoid collisions.

`on("*", handler)` can capture incoming command text, but it can interfere with bot routing. Prefer narrowly named handlers unless a wildcard is the purpose of your library.

## Names and property helpers

`publish(...)` exposes methods under `Libs.Greetings` for this library. It does not create a bare `Greetings` global; the short core names such as `ResLib` are a separate runtime feature.

Inside library source, the injected helpers `setBotProperty`, `getBotProperty`, `setUserProperty`, and `getUserProperty` belong to the library-property API. Keep those names: there are no matching built-in `setBotProp` or `getUserProp` helpers. They pass a library identity, so replacing them with ordinary `Bot.setProp` or `User.getProp` is not a spelling-only change.

## Maintain the contract

Document exported names, parameter types, dependencies, stored properties and callback data. Library code has access to BJS capabilities; installed code should be reviewed like your own commands.

The [official Store library repository](https://github.com/bots-business/store-libs) provides reference implementations. A source file's presence does not establish that an external service still accepts its requests. Test both your library and the service contract before recommending the integration to users.
