# Send HTTP request

Get page on [example.com](http://example.com)

```javascript
  HTTP.get( {
    url: "http://example.com",
    success: '/onLoading',
    error: '/onError'
    
    // if you need pass headers.
    // By default header "content-type" = 'application/json'
    // headers: { "content-type": null }
    // folow_redirects: true, // if you need folow by redirects
    // background: true - if you have timeout error
  } )

/* also you can send POST, PUT, DELETE, OPTIONS requests:
  HTTP.post( {
    url: "http://example.com",
    success: '/onLoading ',
    body: {},  // body params
    // cookies: "" // cookies
    // headers: { "content-type": null } // - if you need headers
    // folow_redirects: true, // if you need folow by redirects
  } )
*/
```

{% hint style="warning" %}
By default header "content-type" is 'application/json'. Some api may have a bug with this. Try set `headers: { "content-type": null }`
{% endhint %}

{% hint style="success" %}
You can use `GET`, `POST`, `PUT`, `DELETE`, `OPTIONS` methods.\
`Http.post`, `HTTP.put` and etc
{% endhint %}

\
\
Command `onLoading`

```javascript
// downloaded page stored on content field
Bot.sendMessage(content);

Bot.inspect(http_status);   // "200"
Bot.inspect(http_headers);  // headers from response
Bot.inspect(cookies); // it is blank for example.com
```

Command `onError`

```javascript
Bot.sendMessage("Error on downloading");

Bot.inspect(http_status);
Bot.inspect(http_headers);  // headers from response
Bot.inspect(cookies);
```

{% hint style="info" %}
Http request can be performed in background with bigger [timeout](../limitations.md).
{% endhint %}

Pass `background: true` if you need request from slow web page. Task on backgroud is more slowly but it have bigger timeout limit.

