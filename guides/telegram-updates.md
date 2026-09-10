---
description: Ask for a contact or location in Telegram, receive photos and files in BJS, and welcome new group members through
  the wildcard command.
---


# Receive contacts, locations, photos and group events

Telegram sends more than text. A shared contact, location, photo or new-member event has its own fields. This guide creates a reply keyboard and a complete receiving command for a [working test bot](../start/first-bot.md).

The example saves contact, location and file references as **user properties in private chats**. It acknowledges receipt without repeating private details into the conversation. Group events only produce a welcome message.

{% stepper %}

{% step %}

### Ask for contact or location <a href="#1-ask-for-contact-or-location" id="1-ask-for-contact-or-location"></a>

In the mobile app, open **Commands** and create `/share`. Leave **Answer** and **Keyboard** empty, **Wait for answer** off and **Auto retry time in seconds** empty. Paste this BJS and tap **Save**:

{% code title="/share" overflow="wrap" %}
```javascript
// Command: /share
if (!user || !chat || chat.chat_type !== "private") { return; }
Api.sendMessage({
  text: "Choose what to share. This test bot saves it in your user properties. Sharing is optional.",
  reply_markup: {
    keyboard: [
      [{ text: "Share my contact", request_contact: true }],
      [{ text: "Share my location", request_location: true }]
    ],
    resize_keyboard: true,
    one_time_keyboard: true
  }
});
```
{% endcode %}

Send `/share` in your private chat with the bot. These request buttons appear below the message input and work only in private chats. They send a contact or location after the user agrees; they do not send their button label as an ordinary text command. [Telegram KeyboardButton](https://core.telegram.org/bots/api#keyboardbutton).

Photos and files use Telegram's attachment control; no special request button is needed.

{% endstep %}

{% step %}

### Receive the update in `*` <a href="#2-receive-the-update-in" id="2-receive-the-update-in"></a>

Create a command named exactly `*`, with the same empty metadata and **Wait for answer** off. If the bot already has `*`, merge the branches below with its existing handling; replacing the whole command would remove its previous behavior.

The routing at the top chooses an action; the named functions below validate and save its data. Copy the complete block, including those functions, into the same command. See [JavaScript functions](../bjs/javascript-basics.md#functions-name-a-reusable-calculation) for the syntax.

{% code title="*" overflow="wrap" %}
```javascript
// Command: *
const incoming = tgUpdate && tgUpdate.message;
if (!incoming || !chat) { return; }

const isGroup = chat.chat_type === "group" || chat.chat_type === "supergroup";
if (isGroup && Array.isArray(incoming.new_chat_members) && incoming.new_chat_members.length) {
  welcomeMembers(incoming.new_chat_members);
  return;
}

// Save personal data only in the sender's private chat.
if (!user || chat.chat_type !== "private") { return; }
if (incoming.contact) { saveContact(incoming.contact); return; }
if (incoming.location) { saveLocation(incoming.location); return; }
if (Array.isArray(incoming.photo) && incoming.photo.length) {
  const photo = incoming.photo[incoming.photo.length - 1];
  saveFile("last_photo_file_id", photo, "Your photo reference was saved.");
  return;
}
if (incoming.document) {
  saveFile("last_document_file_id", incoming.document, "Your file reference was saved.");
  return;
}

// Other updates are ignored. Keep your existing text handling here.

function welcomeMembers(members) {
  const names = members.map(function (member) {
    return member.first_name || member.username || "new member";
  });
  Api.sendMessage({ text: "Welcome, " + names.join(", ") + "!" });
}

function saveContact(contact) {
  if (!contact.user_id || contact.user_id !== user.telegramid ||
      typeof contact.phone_number !== "string" || !contact.phone_number) {
    Bot.sendMessage("Use /share and Share my contact to send your own contact.");
    return;
  }
  User.setProp("shared_contact", {
    phone_number: contact.phone_number,
    telegram_id: contact.user_id
  }, "json");
  Bot.sendMessage("Your contact was saved.");
}

function saveLocation(location) {
  if (!isCoordinate(location.latitude, 90) || !isCoordinate(location.longitude, 180)) { return; }
  User.setProp("shared_location", {
    latitude: location.latitude,
    longitude: location.longitude
  }, "json");
  Bot.sendMessage("Your location was saved.");
}

function isCoordinate(value, limit) {
  return Number.isFinite(value) && Math.abs(value) <= limit;
}

function saveFile(property, file, confirmation) {
  if (!file || !file.file_id) { return; }
  User.setProp(property, file.file_id);
  Bot.sendMessage(confirmation);
}
```
{% endcode %}

A contact may describe somebody else. This example compares its Telegram user ID with `user.telegramid` before saving it as the sender's contact. Both IDs are numbers in this Telegram context, so no `String(...)` conversion is needed; `shared_contact.telegram_id` also stays a number. The explicit `"json"` type keeps the saved object readable with the memory database too. Use `user.id` for APIs that need an internal Bots.Business user ID, not for this comparison. A submitted location does not prove where a person is physically located.

A photo contains several sizes; the example retains the last size's `file_id`. A file sent as a document uses `document.file_id`. These are Telegram file references, not downloaded bytes or public URLs. Do not construct a download URL containing your bot token. See [Telegram Message fields](https://core.telegram.org/bots/api#message).

For complete sending commands, see [Send photos and documents](send-media.md). To reuse files saved by this receiver, use that guide's `/photo` and `/document` commands, replacing `demo_photo_id` with `last_photo_file_id` and `demo_document_id` with `last_document_file_id`. Use the same bot and user.

{% endstep %}

{% step %}

### Test each branch <a href="#3-test-each-branch" id="3-test-each-branch"></a>

1. Send `/share`, tap **Share my contact**, and accept Telegram's prompt. Expect `Your contact was saved.`
2. Send `/share` again and choose **Share my location**. Expect `Your location was saved.`
3. Send a photo through Telegram's attachment control. Expect `Your photo reference was saved.` Send a file as a document and expect the file confirmation.
4. Open your bot's **Chats**, find your private chat and [open **Properties**](../app/properties.md#find-a-property). Check `shared_contact`, `shared_location`, `last_photo_file_id` and `last_document_file_id` after the sending commands have finished. Remove the test data when finished.
5. Forward somebody else's contact. The bot should request your own contact and preserve the previously saved value. Send ordinary text or a sticker; this example should ignore it without a BJS error.

Use a test group to check the welcome branch: add the bot, allow it to send messages, then have another account join. The trigger is the incoming `message.new_chat_members` service message, which reaches `*`; typing `/welcome` does not create that event. A new member does not need a Telegram username. Group visibility rules are described below.

{% endstep %}

{% endstepper %}

## `request`, `tgUpdate` and message text

For an ordinary incoming Telegram message, `request` is the selected Message object, while `tgUpdate` is the original Update envelope. For example, contact data is available as `request.contact` or `tgUpdate.message.contact`. This guide reads the original message from `tgUpdate.message` consistently.

| Incoming update | Fields to inspect |
| --- | --- |
| Shared contact | `tgUpdate.message.contact` |
| Shared location | `tgUpdate.message.location` |
| Photo or document | `tgUpdate.message.photo` or `.document` |
| New group members | `tgUpdate.message.new_chat_members` |
| Ordinary text | `tgUpdate.message.text` |
| Caption on media | `tgUpdate.message.caption` |

A photo's caption is separate from text. The current Bots.Business message dispatcher selects commands from message text, not from a media caption. These updates normally have no command text and reach the wildcard `*`. Never begin a general receiver with an unconditional `message.trim()` or return merely because `message` is empty; that would reject the updates this guide handles.

Inline-button callbacks have a different shape: `request` is the callback query, while `tgUpdate.callback_query` is its envelope field. The guard above deliberately ignores them, edited messages and other update types. See [inline interactions](../bjs/inline.md) and [BJS context](../bjs/context.md).

## Waiting for an answer and group visibility

An active **Wait for answer** can consume a non-text update before `*` runs. Turn waiting off for the two commands above and finish or cancel any existing input flow before testing. A separate known `/cancel` command cancels a pending wait; a contact or location does not cancel it merely by arriving. See [collect input](../app/collect-input.md).

Telegram's privacy mode controls which ordinary group messages a bot receives. A group administrator bot or a bot with privacy mode disabled can receive ordinary user messages more broadly; privacy-enabled bots receive relevant commands and replies. Service messages, including new-member events, are delivered regardless of privacy mode. Bots.Business can process only updates that Telegram actually delivers. This example intentionally saves personal data only in private chats. [Telegram bot visibility FAQ](https://core.telegram.org/bots/faq#what-messages-will-my-bot-get).

If nothing happens, check bot status, the saved `*` command, waiting state, group permissions and **Errors**. Keep other wildcard branches and `@` checks in mind when adapting an existing bot. For replying to a particular message or removing a bot reply later, use [Telegram API examples](../bjs/telegram-api.md).
