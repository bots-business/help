# Send HTTP request

Get page on example.com

```javascript
  HTTP.get( {
    url: "http://example.com",
    success: '/onLoading '
    // headers: headers - if you need headers
  } )

/* also you can send POST request
  HTTP.post( {
    url: "http://example.com",
    success: '/onLoading ',
    body: {},  // body params
    // cookies: "" // cookies   
    // headers: headers - if you need headers
  } )
*/
```

Command onLoading

```javascript
// downloaded page stored on content field
Bot.sendMessage(content)
```



