---
description: With web app you can render web content
cover: ../.gitbook/assets/7b6c8d3366ada2615b.jpeg
coverY: 0
---

# Web App

## Demo bot

{% embed url="https://t.me/BBWebAppBot" %}

## Quickly start

Telegram have [Web Apps](https://core.telegram.org/bots/webapps) now. So BB supports Web Apps too.

{% hint style="danger" %}
**WebApp** is designed for working with HTML, JavaScript, and simple JSON requests. However, if you need to make important changes, such as transferring balances or assigning game points, use [**webhooks**](../libs/webhooks-lib.md) instead.

🔹 [**Webhooks**](../libs/webhooks-lib.md) are secure URLs containing a secret key for validation.\
🔹 **WebApp** is not protected—anyone can send any request to it.

Therefore, for critical operations, always use **webhooks** instead of WebApp.
{% endhint %}

It is possible render text (html, css, js, json...) content to web. For example:

```javascript
// command webExample

// get url for Web page
var url = WebApp.getUrl({ 
  command: "webExample",
  // you can pass options
  // options: { any: "options", here: "possible" } 
});

// we can get page url on bot
Bot.inspect(url);

// html content
var content = "<h1>Hello from " + bot.name + "</h1>"

// render this command in web
WebApp.render({ content: content });
```

Go to bot and sent text `webExample`. You will have link to this page:

![](<../.gitbook/assets/image (92).png>)

###

### Passing data to BJS

It is possible to pass data from web via url params or via options on `WebApp.getUrl`. For example your url is:

api.bots.business/v2/bots/**BOT\_ID**/**web-app**/**index**?secret=SECRET

you can add data with:

api.bots.business/v2/bots/BOT\_ID/web-app/index?secret=SECRE&#x54;**\&key=value\&another\_key=another\_value**

{% hint style="info" %}
**Please note:**

* You can use any text for key or value
* **key** and **value** must be encoded (it can not have this symbols: " ", "?", "&" and others). You can use encodeURIComponent JS method or another accord your language.&#x20;


{% endhint %}



In BJS you can access to this data via options:

```javascript
let key = options.key
let anotherKey = options.another_key

// or more simple:
// let {key, anotherKey } = options
```



### Templates

It is hard to edit html in Java Script. Templates are a good way to organize your web application.

```javascript
// command index
WebApp.render({
  template: "index.html"
  // you can pass mime type also:
  // mime_type: "text/html", // html by default
});
```



{% hint style="success" %}
Possible [mime types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types): `text/css, text/csv, text/javascript, text/css, text/html, application/json and etc`
{% endhint %}

command "`index.html`":

```html
<!-- it is html template. -->
<html>
  <body>
     <h1>Hello from <%bot.name%></h1>
     <!-- Just calc 2+2 -->
     2 + 2 = <% 2+2 %>
  </body>
</html>
```

You can use bjs tag: `<% your BJS code %>` in this template.

We have:

![](<../.gitbook/assets/image (98).png>)

#### Passing Variables

{% hint style="info" %}
**Do not use long code** in bjs tag <% %>

It is bad. You can pass any vars to template
{% endhint %}

For example we can pass CSS file and test variable.&#x20;

Command "`index`":

```javascript
// command index
var botLink = "http://t.me/" + bot.name;
var CSSFile = WebApp.getUrl({ command: "renderCSS" })

WebApp.render({
  template: "index.html",
  options: {
    botLink: botLink,
    CSSFile: CSSFile
  }
});
```

command "`index.html`":

```html
<!-- it is html template. -->
<html>
   <!-- // include template app.css -->
   <head>
     <link rel="stylesheet" href="<% options.CSSFile %> ">
   </head>

   <body>
     <h1>Hello from <a href="<% options.botLink%>"> <%bot.name%></a></h1>
     <!-- Just calc 2+2 -->
     2 + 2 = <% 2+2 %>
   </body>
</html>
```

command "`renderCSS`":

```javascript
WebApp.render({ template: "example.css", mime_type: "text/css" });
```

command "`example.css`":

```css
// It is CSS file
body {
    font-family: 'Share Tech', sans-serif;
    font-size: 15px;
    color: white;
    display: flex;
    jsutify-content: center;
    align-items: center;
    margin: 0;
    width: 100vw;
    height: 100vh;
    text-shadow: 8px 8px 10px #0000008c;
    background-color: #343a40;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='28' height='49' viewBox='0 0 28 49'%3E%3Cg fill-rule='evenodd'%3E%3Cg id='hexagons' fill='%239C92AC' fill-opacity='0.25' fill-rule='nonzero'%3E%3Cpath d='M13.99 9.25l13 7.5v15l-13 7.5L1 31.75v-15l12.99-7.5zM3 17.9v12.7l10.99 6.34 11-6.35V17.9l-11-6.34L3 17.9zM0 15l12.98-7.5V0h-2v6.35L0 12.69v2.3zm0 18.5L12.98 41v8h-2v-6.85L0 35.81v-2.3zM15 0v7.5L27.99 15H28v-2.31h-.01L17 6.35V0h-2zm0 49v-8l12.99-7.5H28v2.31h-.01L17 42.15V49h-2z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E"), linear-gradient(to right top, #343a40, #2b2c31, #211f22, #151314, #000000);
}
 
h1 {
    margin: 40px;
    display: inline-grid;
}
```

Our result now:

![](<../.gitbook/assets/image (82).png>)

## Deployment in Telegram

You can install Web App in [several ways](https://core.telegram.org/bots/webapps#implementing-web-apps)

![](<../.gitbook/assets/image (30).png>)

### Inline button

```javascript
var webExample = WebApp.getUrl({ command: "webExample" });

Api.sendMessage({
  text: "🤑 Try our web page",
  reply_markup: { inline_keyboard: [
    [
      // open the any web page on button pressing
      { text: "open Demo Page (from help)", web_app: { url: webExample } },
    ]
  ]}
});
```

### Keyboard button

```javascript
var webExample = WebApp.getUrl({ command: "webExample" });

Api.sendMessage({
  text: "It is example for BB Web App",

  reply_markup: {
    resize_keyboard: true,
    keyboard: [
      // line 1
      [
        { text: "🤑 Open Web App" },
        { text: "Open App", web_app: { url: webExample } }
      ]
    ]
  }
})
```

{% hint style="success" %}
For another ways please see Telegram [Help](https://core.telegram.org/bots/webapps#implementing-web-apps)
{% endhint %}



## **How to?**

**Why Bot.sendMessage method is not working on Web App?**

Bot.sendMessage - it is method for sending message to user in Telegram. The Web App is not a Telegram chat, so there is no user, chat, etc. - anyone can open this web app, the user can share it with other users, etc.

For sending message you need to pass chat id and use something like AJAX request to bot via [Webhook](../libs/webhooks-lib.md) (it can be more secure) or via another Web App endpoint with [passing data](web-app.md#passing-data-to-bjs).



**How i can make AJAX requests from my Web App?**

This question is now about web development, not bot development. We have jQuery and other methods, libraries (the list is really long). You can include any external js library in your application and use it. Go to the Internet - there is a lot of information there and our help is not the place where this question should be covered



**How to use BB Libs in Web App?**

BB Libs - it is bot libs not Web app Libs. You can not use libs in Web App\


**How to display personal user's data on web page?**

You need to pass this data via url:

```javascript
// command webExample

// get url for Web page
var url = WebApp.getUrl({ 
  command: "webExample",
  // you can pass options
  options: { user: user, chat: chat } 
});

// we can get page url on bot
Bot.inspect(url);
```

So you will have this data in address and can extract it with JavaScript and window.location.search or etc.

If you want more data or if you need any dynamic content - you can use Webhook to bot (or another WebApp-endpoints) + AJAX requests.&#x20;

