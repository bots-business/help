# Google Analytics tracking

Use [Google Analytics](https://analytics.google.com/analytics/web/) for tracking bot statistic.

## Usage

Track  events can be more usefull for bot

```javascript
// Track an Event (all values optional)
GoogleAnalytics.event(ua_key: "UA-XXXXXX-1",
  category: 'refferal', action: 'attracted by', label: user.id, value: 1)
```



```javascript
// Track exceptions (all values optional)
GoogleAnalytics.exception(ua_key: "UA-XXXXXX-1",
  description: 'No money', fatal: false)
```



```javascript
// Track timing (all values optional, but should include time)
// time in milliseconds
GoogleAnalytics.timing(ua_key: "UA-XXXXXX-1",
  category: 'downloading', variable: 'external-api', label: 'coinpayments', time: 50)
```



```javascript
// Track a Pageview (all values optional)
GoogleAnalytics.pageview(ua_key: "UA-XXXXXX-1",
  path: '/vip-area', hostname: 'bots.business', title: 'Vip Area')
```



```javascript
// Track social activity (all values REQUIRED)
GoogleAnalytics.social(ua_key: "UA-XXXXXX-1",
  action: 'like', network: 'bot', target: '/article_1')
```

## Track transactions

```javascript
// Track transaction (transaction_id REQUIRED)
GoogleAnalytics.transaction({
  ua_key: "UA-XXXXXX-1",
  transaction_id: 12345,
  affiliation: 'clothing',
  revenue: 17.98,
  shipping: 2.00,
  tax: 2.50,
  currency: 'EUR'
})
```



```javascript
// Track transaction item (matching transaction_id and item name REQUIRED)
GoogleAnalytics.transaction_item({
  ua_key: "UA-XXXXXX-1",
  transaction_id: 12345,
  name: 'Shirt',
  price: 8.99,
  quantity: 2,
  code: 'afhcka1230',
  variation: 'red',
  currency: 'EUR'
})
```



