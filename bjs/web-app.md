---
description: With web app you can render web content
cover: ../.gitbook/assets/7b6c8d3366ada2615b.jpeg
coverY: 0
---

# Web App

{% hint style="warning" %}
**It is early!**

This function under progress.
{% endhint %}

## Demo bot

{% embed url="https://t.me/BBWebAppBot" %}

## Quickly start

Telegram have [Web Apps](https://core.telegram.org/bots/webapps) now. So BB supports Web Apps too.

It is possible render text (html, css, js, json...) content to web. For example:

```javascript
// command webExample

// get url for Web page
var url = WebApp.getUrl({ command: "webExample" });

// we can get page url on bot
Bot.inspect(url);

// html content
var content = "<h1>Hello from " + bot.name + "</h1>"

// render this command in web
WebApp.render({ content: content });
```

Go to bot and sent text `webExample`. You will have link to this page:

![](<../.gitbook/assets/image (95).png>)

### Templates

It is hard to edit html in Java Script. Templates are a good way to organize your web application.

```javascript
// command index
WebApp.render({ template: "index.html" });
```

and command "`index.html`":

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

![](<../.gitbook/assets/image (91).png>)

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
WebApp.render({ template: "example.css" });
```

command "`example.css`":

```css
// It is CSS file
body {
    font-family: 'Share Tech', sans-serif;
    font-size: 30px;
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

![](<../.gitbook/assets/image (92).png>)
