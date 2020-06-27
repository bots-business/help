# Send HTTP request

Get page on [example.com](http://example.com)

```javascript
  HTTP.get( {
    url: "http://example.com",
    success: '/onLoading',
    error: '/onError'
    // headers: headers - if you need headers
    // background: true - if you have timeout error
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

Command `onLoading`

```javascript
// downloaded page stored on content field
Bot.sendMessage(content)
```

Command `onError`

```javascript
Bot.sendMessage("Error on downloading")
```

{% hint style="info" %}
Http request can be performed in background with bigger [timeout](../limitations.md).
{% endhint %}

Pass `background: true` if you need request from slow web page. Task on backgroud is more slowly but it have bigger timeout limit.



