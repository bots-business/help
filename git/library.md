# Library

You can store common code in the library.

{% hint style="success" %}
See libraries in the Library Store. You can copy any free library and modify it.
{% endhint %}

## Basic

For example: code in file libs\myLib.js:

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

## Commands capturing

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

## Using HTTP

Lib can perform web requests. For example: get page from eample.com and send its content to user.

```javascript
libPrefix = "myLib"

function load(){
  HTTP.get( {
    url: "http://example.com",
    success: libPrefix + 'onLoading '
    // headers: headers - if you need headers
  } )
}

function onLoading(){
   Bot.sendMessage(content);
}

on(libPrefix + 'onLoading', onLoading );
```

on Bot command:

```javascript
Libs.myLib.load();
```



