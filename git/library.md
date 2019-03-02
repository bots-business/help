# Library



For example code in myLib.js:

```javascript
function hello(){
  Bot.sendMessage("Hello from lib!")
}

function goodbye(name){
  Bot.sendMessage("Goodbye, " + name)
}

publish({
  sayHello: hello,
  sayGoodbyeTo: goodbye     
})
```

then you can use Lib in any bot's command:

```javascript
Libs.myLib.hello()
Libs.myLib.sayGoodbyeTo("Alice") 
```

#### Lib - capture command

It is possible to capture command with lib.

For example:

* user type "Hi"
* bot answer "Hello" 

```javascript
function onHiCommand(){
    Bot.sendMessage("Hello");
}

on('Hi', onHiCommand );
```

Master command "\*" - for capture any text from user with lib

```javascript
function onMasterCommand(){
    /// input your code here
}

on('*', onMasterCommand );
```

{% hint style="info" %}
You can use all BJS functions in the Libs 
{% endhint %}

