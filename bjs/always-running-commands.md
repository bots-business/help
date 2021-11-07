# Always running commands

Sometimes code execution is always required.

## Master command

This command executed only when there are no others commands or on [updates](https://core.telegram.org/bots/api#update) for Telegram bot.

Just use `*` in command name.&#x20;

Please see all updates list [here](https://core.telegram.org/bots/api#message)

**Example.**

Command `*`

```javascript
// you can track any message here
if(message){
   Bot.sendMessage("Sorry, bot don't have this command: " + message);
   return
}

// you can see all updated data by:
// Bot.inspect(request);

if(request.edit_date){
  // user edited message
  Bot.sendMessage("Text edited to:" + request.new_text);
  // Please note:
  // we have request.new_text not request.text here
  // for backward compatibility
  // request.text will be nil!
}

// chat title changed
if(request.new_chat_title){
  Bot.sendMessage("New chat title is:" + request.new_chat_title);
}

// another possible updates in:
// https://core.telegram.org/bots/api#message
```

## BeforeAll and AfterAll commands

Code of this commands executed always before (and after) all others commands codes.&#x20;

**Example. **You need add important alert in all commands. You can create only one BeforeAll command with code `Bot.sendMessage("Important alert")`

For `BeforeAll` command use `@` in command name

For `AfterAll` command use `@@` in command name

{% hint style="danger" %}
Please note. Only BJS for `BeforeAll` and `AfterAll` commands runned. No any answer and keyboard here.
{% endhint %}

{% hint style="info" %}
You can share functions, variables and etc with `BeforeAll` and `AfterAll` commands. It is effective for common code parts.
{% endhint %}

```javascript
// code for @ BeforeAll command
function myName(){
  return "Peter"
}
```

```javascript
// code for /test command
Bot.sendMessage(
  myName()  // result will be "Peter"
)

// myName is defined in BeforeAll command
```

{% hint style="danger" %}
Please note. If you need `*`, `@`, `@@ as command names you can use it in aliases`
{% endhint %}

## Return methods.

`return` in BeforeAll command works also for all commands

If you have `return` in any command AfterAll commands do not executed

## Example: Making ban system with BeforeAll command

In command BeforeAll: with `@` name

```javascript
badUsers = Bot.getProperty("badUsers", { list: {} })

if(badUsers.list[user.telegramid]){
  Bot.sendMessage("You are blocked!");
  return // this is worked for all command
  // because it is in BeforeAll command
}
```

In command `/block`:

```javascript
tgID = 1111111;  // any tgID for ban. You can pass it via message with wait for reply
badUsers = Bot.getProperty("badUsers", { list: {} });
badUsers.list[tgID] = true;

// for unban:
// badUsers.list[tgID] = false;

Bot.setProperty("badUsers", badUsers, "json");

Bot.sendMessage("User with TG id: " + tgID + " banned");

// You can also use hard block
// It is save your iterations:
// Bot.blockChat(chat.id);

// But with this BeforeAll will be also not working
```
