---
description: Receive a Telegram photo or document, pass its file_id to another BJS command through options, and send it back
  with a caption or inline button.
---


# Send photos and documents

This recipe receives a file and immediately sends it back through a second command. It passes the file ID in `options`, without creating a user property. You need a running test bot and a private Telegram chat with it. No external file hosting is required.

Create all three commands below before testing. Only `/send-media` waits for a reply; `/photo` and `/document` send the file they receive from it.

{% stepper %}

{% step %}

<a id="save-a-photo-or-document"></a>

### Receive a photo or document <a href="#1-save-a-photo-or-document" id="1-save-a-photo-or-document"></a>

Create `/send-media` with **Answer** `Send a photo or document.` and **Wait for answer** on. Leave **Keyboard** and **Auto retry time in seconds** empty. Its BJS handles the next reply:

{% code title="/send-media" overflow="wrap" %}
```javascript
// Command: /send-media
if (!user || !chat || chat.chat_type !== "private") { return; }
const photos = request && request.photo;
const photo = Array.isArray(photos) ? photos[photos.length - 1] : null;
const document = request && request.document;

if (sendMedia(photo, "/photo")) { return; }
if (sendMedia(document, "/document")) { return; }

Api.sendMessage({ text: "Please attach a photo or document." });
Bot.runCommand("/send-media");

function sendMedia(file, targetCommand) {
  if (!file || typeof file.file_id !== "string" || !file.file_id) { return false; }
  Bot.run({
    command: targetCommand,
    options: { file_id: file.file_id }
  });
  return true;
}
```
{% endcode %}

The helper checks the file ID and passes it to the matching sender. Keep the helper in the same command. A picture attached as a **file** reaches the document branch; a picture sent as a **photo** reaches the photo branch. For a photo, the example uses the last supplied size.

`Bot.run` makes `options.file_id` available in the target command. A new independent message such as `/photo` does not automatically receive that value. If you do not need separate sender commands, you can call `Api.sendPhoto` or `Api.sendDocument` directly here with the received file ID.

{% endstep %}

{% step %}

### Send the photo with a caption and button <a href="#2-send-the-photo-with-a-caption-and-button" id="2-send-the-photo-with-a-caption-and-button"></a>

Create `/photo` with empty **Answer**, **Keyboard** and Auto Retry interval, **Wait for answer** off, and this BJS:

{% code title="/photo" overflow="wrap" %}
```javascript
// Command: /photo
if (!user || !chat || chat.chat_type !== "private") { return; }
const fileId = options && options.file_id;
if (typeof fileId !== "string" || !fileId) {
  Api.sendMessage({ text: "Send /send-media and attach a photo first." });
  return;
}
Api.sendPhoto({
  photo: fileId,
  caption: "Your photo",
  reply_markup: {
    inline_keyboard: [[
      { text: "Open Bots.Business help", url: "https://help.bots.business/" }
    ]]
  }
});
```
{% endcode %}

After receiving a photo, the bot sends it back with its caption and a button below it. You do not need to send `/photo` yourself. The URL button opens the help; it does not run a BJS callback command. For callback actions, see [inline buttons](../bjs/inline.md).

{% endstep %}

{% step %}

### Send the document <a href="#3-send-the-document" id="3-send-the-document"></a>

Create `/document` with empty **Answer**, **Keyboard** and Auto Retry interval, **Wait for answer** off:

{% code title="/document" overflow="wrap" %}
```javascript
// Command: /document
if (!user || !chat || chat.chat_type !== "private") { return; }
const fileId = options && options.file_id;
if (typeof fileId !== "string" || !fileId) {
  Api.sendMessage({ text: "Send /send-media and attach a document first." });
  return;
}
Api.sendDocument({
  document: fileId,
  caption: "Your document"
});
```
{% endcode %}

Use `photo` for `sendPhoto` and `document` for `sendDocument`. Telegram describes the supported inputs and limits in [sendPhoto](https://core.telegram.org/bots/api#sendphoto) and [sendDocument](https://core.telegram.org/bots/api#senddocument).

{% endstep %}

{% endstepper %}

## Test the whole flow

1. Send `/photo` or `/document` manually: the bot should ask you to use `/send-media`, because no file ID was passed.
2. Send `/send-media`, then attach a photo. Check that the bot immediately sends that photo back with its caption and help button.
3. Send `/send-media` again and attach a document. Check the file and caption in the bot's reply.
4. Repeat from another test user: each file must return to its sender. Neither flow should create `demo_photo_id` or `demo_document_id` in user properties.
5. Send ordinary text when asked for a file: the bot should ask again. Sending a known command, such as `/photo`, cancels that wait and runs the command; without `options` it should explain how to start again.
6. After a successful send, enter `/photo` or `/document` in a new message. It should ask for a new attachment rather than reuse a previous file.

## Choose options, parameters or stored data

For a structured handoff, use `Bot.run({ command, options })` as above. The shorter `Bot.runCommand(targetCommand, { file_id: fileId })` passes the same kind of object. Both receivers read `options.file_id`.

Appending an ID to a command name passes text in `params`. For example, `Bot.runCommand("/document " + fileId)` would require changing `/document` to read `params` instead of `options.file_id`. Use one contract consistently. The sender above expects `options`.

Calling `/send-media` again opens another question because its **Wait for answer** is on. To process a known file immediately, call `/photo` or `/document`, with waiting off.

Use a [stored user property](../bjs/user-properties.md) when a user must be able to request the same file in a later, independent message. That is a different flow: deliberately save the ID on receipt, then read it in the later command. The immediate flow above needs no property write or read. Passing to another command still adds a command call; keep the send in a local function if it does not need a separate handler. See [choosing how long data should live](../bjs/context.md#choose-how-long-data-should-live).

If you used the earlier saved-file version of this recipe, update all three commands together and rename its collector `/save-media` to `/send-media`. Existing `demo_photo_id` and `demo_document_id` values are not used or deleted by this version. Remove those demo properties through [Properties](../app/properties.md) once no other commands need them.

Keep a reusable Telegram `file_id`, not `file_unique_id`. File IDs belong to the Telegram bot that received them; a copied project with a different bot token must obtain its own IDs. A local phone path such as `/storage/...` is not a Telegram upload. If you use a URL instead, Telegram must be able to fetch a supported file at that URL.

For a failed send, check **Errors** and the [API callback guide](../bjs/telegram-api.md). Receiving a file ID does not prove that a subsequent send succeeded.

Next: [receive contacts, locations and group events](telegram-updates.md), or [collect text input](../app/collect-input.md).
