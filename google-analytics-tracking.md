# تتبع تحليلات جوجل

استعمال [Google Analytics](https://analytics.google.com/analytics/web/) for tracking bot statistic.

## استعمال

يمكن أن تكون أحداث المسار أكثر فائدة للبوت

```javascript
// تتبع حدث (جميع القيم اختيارية)
GoogleAnalytics.event({
  ua_key: "UA-XXXXXX-1",
  category: 'refferal',
  action: 'attracted by',
  label: user.id,
  value: 1
})
```



```javascript
// تتبع الاستثناءات (جميع القيم اختياري)
GoogleAnalytics.exception({
  ua_key: "UA-XXXXXX-1",
  description: 'No money',
  fatal: false
})
```



```javascript
// توقيت المسار (جميع القيم اختيارية ، ولكن يجب أن تشمل الوقت)
// الوقت بالميلي ثانية
GoogleAnalytics.timing({
  ua_key: "UA-XXXXXX-1",
  category: 'downloading',
  variable: 'external-api',
  label: 'coinpayments',
  time: 50
})
```



```javascript
// تتبع صفحة (جميع القيم اختياري)
GoogleAnalytics.pageview({
  ua_key: "UA-XXXXXX-1",
  path: '/vip-area',
  hostname: 'bots.business',
  title: 'Vip Area'
})
```



```javascript
// تتبع النشاط الاجتماعي (جميع القيم المطلوبة)
GoogleAnalytics.social({
  ua_key: "UA-XXXXXX-1",
  action: 'like',
  network: 'facebook',
  target: '/article_1'
})
```

### تتبع المعاملات

```javascript
// تتبع المعاملات
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
// تتبع عنصر المعاملة
// (مطابقة المعاملة_المطلوب واسم العنصر مطلوب)
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

### خيارات "عالمية"

أي من الخيارات في قائمة المعلمات \([https://developers.google.com/analytics/devguides/collection/protocol/v1/parameters](https://developers.google.com/analytics/devguides/collection/protocol/v1/parameters)\) that are accepted on ALL hit types can be set as options on any of the hits.

```javascript
GoogleAnalytics.pageview({ path: '/video/1235', user_id: user.id })
```

معرف المستخدم هو خيار عالمي في المثال أعلاه.

القائمة الكاملة في هذا الوقت:

```text
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

خيارات منطقية مثل
`anonymize_ip` سيتم تحويل من `true`/`false` into `1`/`0` وفقًا لمستندات تتبع التتبع.



### **ضرب غير التفاعلية**

```javascript
# Track a Non-Interactive Hit
GoogleAnalytics.event({ category: 'webhook', action: 'send', non_interactive: true })
```

تعد الأحداث غير التفاعلية مفيدة لتتبع أشياء مثل رسائل البريد الإلكتروني المرسلة ، أو الأحداث الأخرى التي لا تكون مباشرة نتيجة تفاعل المستخدم.

الخيار
`non_interactive` مقبول لجميع الطرق على `tracker`.

### **التحكم في الجلسة**

```javascript
// بدء جلسة
GoogleAnalytics.pageview({ path: '/blog', start_session: true })

// انهاء الجلسة
GoogleAnalytics.pageview({ path: '/blog', end_session: true })
```

الخيارات الأخرى مقبولة لبدء الجلسة وإنهاؤها: `session_start`, `session_end`, and `stop_session`.

### **تجربة المحتوى**

```javascript
// تتبع تجربة
// مفيد لتتبع اختبار A / B أو متعدد المتغيرات
GoogleAnalytics.pageview({
  path: '/blog',
  experiment_id: 'a7a8d91df',
  experiment_variant: 'a'
})
```

