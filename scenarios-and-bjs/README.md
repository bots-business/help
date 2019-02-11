# Scenarios and BJS

### What are these scenarios in the command?

Command can have some scenarios with BJS code. 

{% hint style="info" %}
BJS - it is "Bot Java Script" code.

It is Java Script!
{% endhint %}

Example. Calculating `2+2` and send result to the chat:

```javascript
Bot.sendMessage(2+2);
```

On command executing, its scenarios are executed sequentially and isolated.



{% hint style="success" %}
In BJS, you can use all the usual JS functions except setTimeout, setInterval
{% endhint %}



### Why can there be several BJS scenarios in one command?

In one command, you can download multiple http addresses. Each such download requires a separate scenario.

**In the BJS scenario, you can specify the URL to download.** In this case, the rest of the code will be executed after the download:

```javascript
->(http://example.com)
Bot.sendMessage("Content from example.com:\n" + data.content);
```

Here the page loads from example.com and sends its html content to the chat.



### How can I set the url address

**You can pass a variable to the URL of the previous script.** This is useful if you need to generate a URL.

```text
->(<my_url>)
```

The variable `my_url` needs to be set in the previous script of this command:

```javascript
Command.setValue("my_url", "http://example.com");

->(<my_url>)
```

Here in the first scenario, the variable `my_url` is set. In the second scenario, the page is loaded from [http://example.com](http://example.com/).

> You can set `my_url` in any other command with function `Command.setValue`

### 

### How to create multiple scripts in a command?

Scenarios are separated with `-> (url)` If substring `->(url)` is not found - then only one scenario is created for the command.

