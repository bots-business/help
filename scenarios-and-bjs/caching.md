# Caching

Commands must be executed quickly within 100 - 250 ms. If commands take a long time to complete, users have a bad impression of using the bot

Some commands cannot be executed quickly:

* HTTP loading
* long api requests \(for example, [sendVideo](https://core.telegram.org/bots/api#sendvideo), [getChatMember](https://core.telegram.org/bots/api#getchatmember)- all get methods and etc\)
* mass message broadcasting
* Bot.runAll

Some commands can be very large or poorly coded:

* mass properting reading/setting \(more then 20 properties in one command
* big \(or infinite!\) loops
* etc

**if your code runs slower than 300ms - necessary:**

* try to make it more quickly
* or cache it
* or run command on background and cache it \(if needed\)

{% hint style="danger" %}
If your code takes very long time it will be aborted with timeout error
{% endhint %}

{% hint style="danger" %}
Slow commands can cause your Cloud to **degrade and even crash.**

For example, let's say your cloud can process 1 request per second.

Thus, 4 requests of 250 ms each will be processed per second

But if you have slow commands \(let's say one second\), the cloud will process only one command instead of four.

Thus, the response to the first message will only be sent in a second. And the answer to the last is the fourth already in four.
{% endhint %}

## Caching methods

### setCache

Example set command caching for 1 hour \(for all bot users\):

```javascript
Bot.sendMessage("Hello!");

Bot.setCache(
  60*60 // time in seconds 60*60 = 1 hour
)
```

Example of command caching for 1 hour \(for one user only\):

```javascript
User.sendMessage("Hello, " + user.first_name);

User.setCache(
  60*60 // time in seconds 60*60 = 1 hour
)
```

### clearCache

you can clear Cache \(it can be usefull on changing data\)

```javascript
Bot.clearCache(
  "/command" // command Name
)
```

clear for user caching

```javascript
User.clearCache(
  "/command" // command Name
)
```

## Advanced techniques

You can PRE run caching in background for long command.

Command `/start`

```javascript
Bot.sendMessage("Welcome to my bot");

Bot.run({
  command: "/longBackroundTask",
  run_after: 1 // will be runned in background after 1 sec
})
```

Command /longBackroundTask

```javascript
// make your long task here...

User.setCache(
  60*60*24*7 // time in seconds - 60*60*24*7 it is week
)
```

