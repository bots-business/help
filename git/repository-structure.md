# Repository structure

{% hint style="success" %}
You can make export for any free bot in the Store. Just install it. 

It is good for practice.
{% endhint %}

### bot.json file

Please see [this](https://help.bots.business/git/file-bot-json)

### Commands - in commands folder

File name - it is command name \(But it can be rewritten in command description\)

{% hint style="warning" %}
For commands with "/" \(for example command "/start"\) file name is "\_start"
{% endhint %}

Command can have: `name`, `help`, `aliases` \(second names\), `answer`, `keyboard`, `scnarios` \(for simple logic\) and other options.

{% hint style="success" %}
If the command has a folder - it is located in a folder on the disk with the same name
{% endhint %}

#### Command description

It is optional file header:

```javascript
/*CMD  command: /test  help: this is help for ccommand  need_reply: [ true or false here ]  auto_retry_time: [ time in sec ]  folder: MyFolder  answer: it is example answer for /test command  keyboard: button1, button2  aliases: /test2, /test3CMD*/
```

{% hint style="info" %}
Command description - it is optional block.
{% endhint %}

multiline also supported. For example for answer:

```javascript
/*CMD    <<ANSWERtest answerwith severallines  ANSWERCMD*/
```

{% hint style="info" %}
You can have only answer \(or others\) key in the command description. All keys - optional
{% endhint %}

See [more](https://help.bots.business/commands)

#### Command body

It is command code in JavaScript. Use Bot Java Script for logic in command.

For example:

`Bot.sendMessage(2+2);`

See [more](https://help.bots.business/scenarios-and-bjs)



### Libraries - in libs folder

You can store common code in the libs folder

See [more](https://help.bots.business/git/library)



