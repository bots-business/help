# تتبع تحليلات جوجل

استعمال
[Google Analytics](https://analytics.google.com/analytics/web/) لتتبع إحصائية بوت.

## استعمال

يمكن أن تكون أحداث المسار أكثر فائدة للبوت

```javascript
// Track an Event (جميع القيم اختياري)
GoogleAnalytics.event(ua_key: "UA-XXXXXX-1",
  category: 'refferal', action: 'attracted by', label: user.id, value: 1)
```



```javascript
// Track exceptions (جميع القيم اختياري)
GoogleAnalytics.exception(ua_key: "UA-XXXXXX-1",
  description: 'No money', fatal: false)
```



```javascript
// Track timing (جميع القيم اختيارية ، ولكن يجب أن تشمل الوقت)
// الوقت بالميلي ثانية
GoogleAnalytics.timing(ua_key: "UA-XXXXXX-1",
  category: 'downloading', variable: 'external-api', label: 'coinpayments', time: 50)
```



```javascript
// Track a Pageview (جميع القيم اختياري)
GoogleAnalytics.pageview(ua_key: "UA-XXXXXX-1",
  path: '/vip-area', hostname: 'bots.business', title: 'Vip Area')
```



```javascript
// Track social activity (جميع القيم المطلوبة)
GoogleAnalytics.social(ua_key: "UA-XXXXXX-1",
  action: 'like', network: 'bot', target: '/article_1')
```

## تتبع المعاملات

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



