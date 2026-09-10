---
description: Display names with CommonLib, generate random values, format UTC dates, and calculate hashes using BJS utilities.
---

# Random values, dates and hashes

These are separate tasks with different tools. JavaScript's `Math` and `Date`, plus runtime `CryptoJS`, are available without a Store library. `Random` and `DateTimeFormat` are installed libraries.

## Display a user's name

`CommonLib` is included in BJS. It is the shorter name for `Libs.CommonLib` and `Libs.commonLib`; no installation is required.

```javascript
if (!user) { return; }
const name = CommonLib.getNameFor(user) || "there";
Bot.sendMessage("Hello, " + name + "!", { parse_mode: null });
```

The helper prefers `@username`, then the first name, then the last name, and returns an empty string when none is available. The fallback above uses `there` in that case. It chooses display text, not an authorization identity.

## Choose a random reply

```javascript
const replies = ["Hello!", "Welcome!", "Good to see you!"];
const index = Math.floor(Math.random() * replies.length);
Bot.sendMessage(replies[index]);
```

For a die result:

```javascript
const roll = 1 + Math.floor(Math.random() * 6);
Bot.sendMessage("You rolled " + roll);
```

Use this for casual variation, not secret tokens or decisions requiring cryptographic randomness.

### Compatibility with the Random library

The published `Random` source exposes `randomInt(min, max)`, `randomFloat(min, max)` and `sendMessage(messages)`. Its checked implementation rejects a zero boundary and uses `max - min + 1` for floating point values. Its message picker calls the integer function with zero, so the old `Libs.Random.sendMessage` recipe is not a reliable example for that version.

Use the plain JavaScript recipes above for new code. If your installed version differs, inspect it before depending on different behavior. Do not silently assume that `randomFloat` stays below the supplied maximum.

## Format a date

For an unambiguous timestamp:

```javascript
Bot.sendMessage(new Date().toISOString());
```

For a custom UTC display, install `DateTimeFormat`:

```javascript
const text = Libs.DateTimeFormat.format(
  new Date(),
  "yyyy-mm-dd HH:MM:ss",
  true
);
Bot.sendMessage(text + " UTC");
```

In this library, lowercase `mm` means month and uppercase `MM` means minutes. The third argument selects UTC. An invalid date throws. Do not infer the user's timezone from the server clock; store an explicit preference if your bot needs local-time schedules.

## Calculate a hash or HMAC

`CryptoJS` is provided by the runtime:

```javascript
const digest = CryptoJS.SHA256("example message").toString(CryptoJS.enc.Hex);
Bot.sendMessage(digest);
```

An HMAC uses a shared secret:

```javascript
const signature = CryptoJS.HmacSHA256(
  "example payload",
  "example-secret-for-a-local-test"
).toString(CryptoJS.enc.Hex);
Bot.sendMessage(signature);
```

For provider callbacks, use the exact required algorithm, original payload bytes and output encoding. Re-serializing parsed JSON may change the signature. Hashing is not encryption; an ordinary hash of a password is not a password storage scheme. See the [CryptoJS reference](https://cryptojs.gitbook.io/docs) for supported encodings and methods.

For an actual external callback flow, continue with [Webhooks](webhooks.md) or [OxaPay](../integrations/oxapay.md).
