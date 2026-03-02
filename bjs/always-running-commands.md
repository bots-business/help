# Always running commands

Sometimes code execution is always required.

## Master command

This command executed only when there are no others commands or on [updates](https://core.telegram.org/bots/api#update) for Telegram bot.

Use `*` in command name.&#x20;



### All Updates

{% hint style="success" %}
**You can handle** [**updates**](https://core.telegram.org/bots/api#update) **with Master Command**

It is possible via `tgUpdate` [variable](variables.md).
{% endhint %}

For inspect data you can use:

`throw new Error(inspect(`tgUpdate`))`

then go to Error Tab and see data. Now you can use it via `tgUpdate.xxx.yyy`



### **Example**

Command `*`

```javascript
// you can track any message here
if(message&&chat){
   Bot.sendMessage("Sorry, bot don't have this command: " + message);
   return
}

// you can see all updated data by:
// Bot.inspect(tgUpdate);

if(tgUpdate.edited_message?.edit_date){
  // user edited message
  Bot.sendMessage("Text edited to:" + tgUpdate.edited_message.text);
}

// another possible updates in:
// https://core.telegram.org/bots/api#update
```

## BeforeAll and AfterAll commands

Code of this commands executed always before (and after) all others commands codes.&#x20;

**Example.** You need add important alert in all commands. You can create only one BeforeAll command with code `Bot.sendMessage("Important alert")`

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

##
