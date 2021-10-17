# Google Analytics tracking

Use [Google Analytics](https://analytics.google.com/analytics/web/) for tracking bot statistic.

## Usage

Track  events can be more usefull for bot

```javascript
// Track an Event (all values optional)
GoogleAnalytics.event({
  ua_key: "UA-XXXXXX-1",
  category: 'refferal',
  action: 'attracted by',
  label: user.id,
  value: 1
})
```



```javascript
// Track exceptions (all values optional)
GoogleAnalytics.exception({
  ua_key: "UA-XXXXXX-1",
  description: 'No money',
  fatal: false
})
```



```javascript
// Track timing (all values optional, but should include time)
// time in milliseconds
GoogleAnalytics.timing({
  ua_key: "UA-XXXXXX-1",
  category: 'downloading',
  variable: 'external-api',
  label: 'coinpayments',
  time: 50
})
```



```javascript
// Track a Pageview (all values optional)
GoogleAnalytics.pageview({
  ua_key: "UA-XXXXXX-1",
  path: '/vip-area',
  hostname: 'bots.business',
  title: 'Vip Area'
})
```



```javascript
// Track social activity (all values REQUIRED)
GoogleAnalytics.social({
  ua_key: "UA-XXXXXX-1",
  action: 'like',
  network: 'facebook',
  target: '/article_1'
})
```

### Track transactions

```javascript
// Track transaction
// (transaction_id REQUIRED)
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
// Track transaction item
// (matching transaction_id and item name REQUIRED)
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

### "Global" Options

Any of the options on the parameters list ([https://developers.google.com/analytics/devguides/collection/protocol/v1/parameters](https://developers.google.com/analytics/devguides/collection/protocol/v1/parameters)) that are accepted on ALL hit types can be set as options on any of the hits.

```javascript
GoogleAnalytics.pageview({ path: '/video/1235', user_id: user.id })
```

User id is a global option in the example above.

The complete list at this time:

```
 anonymize_ip,
 queue_time,
 data_source,
 cache_buster,
 user_id,
 user_ip,
 user_agent,
 referrer,
 campaign_name,
 campaign_source,
 campaign_medium,
 campaign_keyword,
 campaign_content,
 campaign_id,
 adwords_id,
 display_ads_id,
 screen_resolution,
 viewport_size,
 screen_colors,
 user_language,
 java_enabled,
 flash_version,
 document_location,
 document_encoding,
 document_hostname,
 document_path,
 document_title,
 link_id,
 application_name,
 application_version,
 application_id,
 application_installer_id,
 experiment_id,
 experiment_variant,
 product_action,
 product_action_list,
 promotion_action,
 geographical_id
```

Boolean options like `anonymize_ip` will be converted from `true`/`false` into `1`/`0` as per the tracking API docs.



### **Non-Interactive Hit**

```javascript
# Track a Non-Interactive Hit
GoogleAnalytics.event({ category: 'webhook', action: 'send', non_interactive: true })
```

Non-Interactive events are useful for tracking things like emails sent, or other events that are not directly the result of a user's interaction.

The option `non_interactive` is accepted for all methods on `tracker`.

### **Session Control**

```javascript
// start a session
GoogleAnalytics.pageview({ path: '/blog', start_session: true })

// end a session
GoogleAnalytics.pageview({ path: '/blog', end_session: true })
```

Other options are acceptable to start and end a session: `session_start`, `session_end`, and `stop_session`.

### **Content Experiment**

```javascript
// Tracking an Experiment
//   useful for tracking A/B or Multivariate testing
GoogleAnalytics.pageview({
  path: '/blog',
  experiment_id: 'a7a8d91df',
  experiment_variant: 'a'
})
```
