# Always running commands

Sometimes code execution is always required.

## Master command

This command executed only when there are no others commands.

Just use `*` in command name. 

## BeforeAll and AfterAll commands

Code of this commands executed always before \(and after\) all others commands codes. 

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

