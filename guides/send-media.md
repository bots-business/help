---
description: Receive a Telegram photo or document, save its file_id, and send it again with a caption or inline button using
  BJS.
---


# Send photos and documents

This recipe saves a file sent to your bot and sends it back on demand. You need a running test bot and a private Telegram chat with it. No external file hosting is required.

{% stepper %}

{% step %}

### Save a photo or document <a href="#1-save-a-photo-or-document" id="1-save-a-photo-or-document"></a>

Create `/save-media` with **Answer** `Send a photo or document to save for this demo.` and **Wait for answer** on. Leave Keyboard empty. Its BJS handles the next reply:

{% code title="/save-media" overflow="wrap" %}
```javascript
// Command: /save-media
if (!user || !chat || chat.chat_type !== "private") { return; }
var photos = request && request.photo;
var document = request && request.document;
if (Array.isArray(photos) && photos.length > 0) {
  var photo = photos[photos.length - 1];
  if (photo && typeof photo.file_id === "string") {
    User.setProp("demo_photo_id", photo.file_id);
    Api.sendMessage({ text: "Photo saved. Send /photo to see it again." });
    return;
  }
}
if (document && typeof document.file_id === "string") {
  User.setProp("demo_document_id", document.file_id);
  Api.sendMessage({ text: "Document saved. Send /document to receive it again." });
  return;
}
Api.sendMessage({ text: "Please attach a photo or document." });
Bot.runCommand("/save-media");
```
{% endcode %}

Each user has their own saved photo and document. Sending a new file of the same kind replaces that user's saved ID. A picture attached as a **file** reaches the document branch; a picture sent as a **photo** reaches the photo branch.

{% endstep %}

{% step %}

### Send the photo with a caption and button <a href="#2-send-the-photo-with-a-caption-and-button" id="2-send-the-photo-with-a-caption-and-button"></a>

Create `/photo` with an empty Answer and Keyboard, **Wait for answer** off, and this BJS:

{% code title="/photo" overflow="wrap" %}
```javascript
// Command: /photo
if (!user || !chat || chat.chat_type !== "private") { return; }
var fileId = User.getProp("demo_photo_id");
if (typeof fileId !== "string" || !fileId) {
  Api.sendMessage({ text: "Send /save-media and attach a photo first." });
  return;
}
Api.sendPhoto({
  photo: fileId,
  caption: "Your saved photo",
  reply_markup: {
    inline_keyboard: [[
      { text: "Open Bots.Business help", url: "https://help.bots.business/" }
    ]]
  }
});
```
{% endcode %}

Telegram should show the photo, its caption and a button below it. The URL button opens the help; it does not run a BJS callback command. For callback actions, see [inline buttons](../bjs/inline.md).

{% endstep %}

{% step %}

### Send the document <a href="#3-send-the-document" id="3-send-the-document"></a>

Create `/document` with empty Answer and Keyboard, **Wait for answer** off:

{% code title="/document" overflow="wrap" %}
```javascript
// Command: /document
if (!user || !chat || chat.chat_type !== "private") { return; }
var fileId = User.getProp("demo_document_id");
if (typeof fileId !== "string" || !fileId) {
  Api.sendMessage({ text: "Send /save-media and attach a document first." });
  return;
}
Api.sendDocument({
  document: fileId,
  caption: "Your saved document"
});
```
{% endcode %}

Use `photo` for `sendPhoto` and `document` for `sendDocument`. Telegram describes the supported inputs and limits in [sendPhoto](https://core.telegram.org/bots/api#sendphoto) and [sendDocument](https://core.telegram.org/bots/api#senddocument).

{% endstep %}

{% endstepper %}

## Test the whole flow

1. Send `/photo` before saving anything: the bot should explain the prerequisite.
2. Send `/save-media`, attach a photo, then send `/photo`: check the image, caption and help button.
3. Send `/save-media` again, attach a document, then send `/document`: check the file and caption.
4. Repeat from another test user: the first user's file must not appear in the second user's saved state.
5. Send ordinary text when asked for a file: the bot should ask again. Sending a known command, such as `/photo`, cancels that wait and runs the command.

Keep a reusable Telegram `file_id`, not `file_unique_id`. File IDs belong to the Telegram bot that received them; a copied project with a different bot token must obtain its own IDs. A local phone path such as `/storage/...` is not a Telegram upload. If you use a URL instead, Telegram must be able to fetch a supported file at that URL.

For a failed send, check **Errors** and the [API callback guide](../bjs/telegram-api.md). Saving an ID confirms receipt of the input; it does not prove a later send succeeded.

Next: [receive contacts, locations and group events](telegram-updates.md), or [collect text input](../app/collect-input.md).
