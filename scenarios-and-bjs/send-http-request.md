# Send HTTP request

Get page on [example.com](http://example.com)

```javascript
  HTTP.get( {    url: "http://example.com",    success: '/onLoading',    error: '/onError'    // headers: headers - if you need headers  } )/* also you can send POST request  HTTP.post( {    url: "http://example.com",    success: '/onLoading ',    body: {},  // body params    // cookies: "" // cookies       // headers: headers - if you need headers  } )*/
```

Command `onLoading`

```javascript
// downloaded page stored on content fieldBot.sendMessage(content)
```

Command `onError`

```javascript
Bot.sendMessage("Error on downloading")
```

