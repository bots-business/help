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





