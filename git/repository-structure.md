# Repository structure

{% hint style="success" %}
You can make export for any free bot in the Store. Just install it. 

It is good for practice.
{% endhint %}

### bot.json file

Please see [this](https://help.bots.business/git/file-bot-json)

### Commands - in commands folder

File name - it is command name \(Bot it can be rewritten in command description\)

Command can have: `name`, `help`, `aliases` \(second names\), `answer`, `keyboard`, `scnarios` \(for simple logic\) and other options.

#### Command description

It is file header:

```javascript
/*CMD
  command: /test
  help: this is help for ccommand
  need_reply: [ true or false here ]
  auto_retry_time: [ time in sec ]
  answer: it is example answer for /test command
  keyboard: button1, button2
  aliases: /test2, /test3
CMD*/
```

See [more](https://help.bots.business/commands)

#### Command body

It is command code in JavaScript. Use Bot Java Script for logic in command.

For example:

`Bot.sendMessage(2+2);`

See [more](https://help.bots.business/scenarios-and-bjs)



### Libraries - in libs folder

You can store common code in the libs folder

See [more](https://help.bots.business/git/library)

























