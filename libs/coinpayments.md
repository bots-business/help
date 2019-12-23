# CoinPayments \(CP\)

هذا ليب جعل التكامل مع [https://www.coinpayments.net](https://www.coinpayments.net/index.php?ref=5418303a5fc165090ee8a9177a3982de) in easy way.

## الإعداد الأولي

تحتاج إلى إعداد المفتاح العام والخاص:

1. [Register](https://www.coinpayments.net/index.php?ref=5418303a5fc165090ee8a9177a3982de)
2. Go to this [page](https://www.coinpayments.net/acct-api-keys) وتوليد مفتاح جديد.

![](../.gitbook/assets/image%20%2847%29.png)

اضغط على زر "تحرير الأذونات" وأضف أذونات مفتاح واجهة برمجة التطبيقات:

![Check all options what you need](../.gitbook/assets/image%20%2818%29.png)

ثم على الروبوت
`/ setup` command:

```javascript
// الحصول على المفاتيح الخاصة بك في https://www.coinpayments.net/index.php?cmd=acct_api_keys
Libs.CoinPayments.setPrivateKey("YOUR KEY");
Libs.CoinPayments.setPublicKey('YOUR KEY');

// لتلقي المدفوعات
// احصل على مفتاح BB Api من تطبيق Bots.Business في الملف الشخصي
Libs.CoinPayments.setBBApiKey('YOUR API KEY');
```

## استدعاء أساليب API

كل طريقة CoinPayments API متاحة [here](https://www.coinpayments.net/apidoc-intro).

على سبيل المثال للطريقة
[Get Basic Account Information](https://www.coinpayments.net/apidoc-get-basic-info) we need 2 commands: `/info` and `/onInfo`

`/info` command:

```javascript
Libs.CoinPayments.apiCall({
  fields: { cmd: "get_basic_info"},
  onSuccess: '/onInfo'
});
```

{% hint style="info" %}
في الحقول ، يمكنك تمرير جميع الحقول من CoinPayments api. فقط اقرأ [help](https://www.coinpayments.net/apidoc-intro). 
{% endhint %}

`/onInfo` command:

```javascript
Bot.sendMessage(inspect(options));
Bot.sendMessage("CoinPayments owner email:" + options.body.result.email);
```

## Combine libs!

لا تملك CoinPayments API بعض الطرق. على سبيل المثال الحصول على التوازن حسب العنوان ، والتحقق من صحة العنوان ، والحصول على المعاملات للعنوان وغيرها.

Use Block.io [Lib](https://help.bots.business/libs/blockio) with CP Lib together!

Block.io مجاني إذا كنت لا تستخدم محافظ هناك.



## تلقي المدفوعات

{% hint style="success" %}
See [demo bot](https://telegram.me/BBDemoStoreBot). Available in the Store.
{% endhint %}

من الممكن تلقي الدفع مقابل محفظة مؤقتة أو دائمة.

**فوائد المحفظة المؤقتة:**

* مبلغ ثابت
* يمكن ربط الدفع إلى المنتج المطلوب
* الحالة والخروج صفحة
* رمز الاستجابة السريعة للدفع
* عنوان واحد للدفع واحد

** فوائد المحفظة الدائمة: **

* أي كمية
* عنوان واحد 

### الإعداد: ضبط IPN Secret

{% hint style="warning" %}
The first step is to go to the [My Settings](https://www.coinpayments.net/index.php?cmd=acct_settings) صفحة> ** إعدادات التاجر ** وتعيين IPN Secret.

يعد IPN Secret الخاص بك سلسلة من ** اختيارك **. يوصى أن تكون سلسلة عشوائية من الحروف والأرقام والأحرف الخاصة.

CoinPayments
** لن ترسل **
أي IPNs ما لم يكن لديك مجموعة IPN السرية. 

See [more](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

** واحد أكثر! ** تحتاج إلى إدخال ** أي نص
\(random text\)** سر IPN في صفحة إعدادات التاجر

## محفظة مؤقتة

نحن نستخدم الأمر "create \ _transaction" مع IPN.

لطفا أنظر [https://www.coinpayments.net/apidoc-create-transaction](https://www.coinpayments.net/apidoc-create-transaction) for details.

نعم ، يمكنك كتابتها عبر `Libs.CoinPayments.apiCall`method too. But هناك طريقة أسهل.

### 

### Command `/pay`

```javascript
let amount = 0.0001; // المبلغ في BTC

options = {
  fields: {
     amount: amount,   // المبلغ في BTC
     currency: "BTC",  // عملة 1 = عملة2 = BTC
     // currency1: "BTC",   // العملة الأصلية للمعاملة
     // currency2: "LTC"  //العملة التي سيرسلها المشتري
     // يمكنك استخدام حقول أخرى أيضا
     // باستثناء العرف و ipn_url (يستخدمه Lib)
     // نرى https://www.coinpayments.net/apidoc-create-transaction
  },
  // محفظة ولدت ، رمز الاستجابة السريعة ، صفحة الدفع
  // سوف تكون متاحة في هذا الأمر
  onSuccess: '/onCreatePayment',
  
  // على الدفع الناجح هذا الأمر
  // سيتم تنفيذها
  onPaymentCompleted: "/onPaymentCompleted",
  
  // ليست ضرورية
  // onIPN: "/onIPN"
  
  // إذا كنت تريد تخصيص رسائل الخطأ
  // onError: "/onError"
}

Libs.CoinPayments.createTransaction(options);
```

{% hint style="success" %}
#### تلقائيا مع المكتبة:

#### CoinPayments: [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

{% hint style="info" %}
من الأفضل استخدام الطريقة ** `onPaymentCompleted` ** وليس الطريقة **` onIPN` **. 

نظرًا لأن الطريقة ** `onPaymentCompleted` ** تغطي IPN بالكامل وتحل المشكلة [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

### Command `/onCreatePayment`

```javascript
// يمكنك فحص جميع الخيارات:
// Bot.sendMessage(inspect(options));

let result = options.result;

let msg = "*Need pay:*\n `" + result.amount + "`" + 
 "\n\n*to address:*\n" +
 "`" + result.address + "`" +
 "\n\n [Checkout](" + result.checkout_url +
    ") | [Status](" + result.status_url + 
 ")" // يمكنك uncomment هذا لفحص الوضع اليدوي
 // + "\n\nCheck status manually: /check" + options.payment_index;

Bot.sendMessage(msg);
Api.sendPhoto({ photo: result.qrcode_url }); 

```

### Command `/onPaymentCompleted`

سيتم تنفيذ هذا الأمر عند الدفع بنجاح

{% hint style="info" %}
تحتاج تثبيت ResourcesLib
{% endhint %}

```javascript
// يمكنك فحص جميع الخيارات
// Bot.sendMessage(inspect(options));

if(!options){
   للأمن ، نحتاج إلى التحقق من أن هذا الأمر لا يعمل إلا من خلال lib
   // المستخدم لا يمكن تشغيل الأمر مع الخيارات
   return
}

Bot.sendMessage("Payment completed");

اسمحوا المبلغ = options.amount1 ؛

let res = Libs.ResourcesLib.userRes("balance");
res.add(amount)

Bot.sendMessage("added to balance, BTC: " + amount);
```

### تم الانتهاء من!

الآن يمكنك تلقي المدفوعات

### معلومة اضافية

يمكنك التحقق من حالة الدفع

```javascript
Libs.CoinPayments.getTxInfo({
  payment_index: payment_index,  // see /onCreatePayment command.
                                    // انت بحاجة تمرير هذا payment_index إلى هذا الأمر
  onSuccess: '/on_txn_id'
})
```

#### `command: /on_txn_id:` 

```javascript
// يمكنك فحص جميع الخيارات:
// Bot.sendMessage(inspect(options));

Bot.sendMessage(options.result.status_text);

// لا تنتهي الدفع هنا.
// Use /onPaymentCompleted for this
```

#### `command /onIPN`

يمكنك الحصول على معلومات من IPN. حقا ليست هناك حاجة في بسيطة. فقط استخدم خيار onPaymentCompleted على createTransaction.

```javascript
// يمكنك فحص جميع المجالات:
// Bot.sendMessage(inspect(options))

// IPN is not needed
// Use - onPaymentCompleted

Bot.sendMessage("IPN: Payment status: " + options.status_text );

```

#### `command onError`

```javascript
// يمكنك فحص جميع المجالات:
Bot.sendMessage(inspect(options))
```

### 

## محفظة دائمة

نحن نستخدم القيادة
"get\_callback\_address" with IPN.

لطفا أنظر [https://www.coinpayments.net/apidoc-get-callback-address](https://www.coinpayments.net/apidoc-get-callback-address) للتفاصيل.

نعم ، يمكنك كتابتها عبر `Libs.CoinPayments.apiCall`method too. But there is an easier way.



### Command `/createWallet`

```javascript
Libs.CoinPayments.createPermanentWallet({
  currency: "BTC",
  //label: "myLabel",
  onSuccess: "/onWalletCreate",
  
  // onIPN - not necessary
  //onIPN: "/onPermanentWalletIPN",
  
  onIncome: "/onIncome"
  
  // إذا كنت تريد تخصيص رسائل الخطأ
  // onError: "/onError"
});
```

{% hint style="success" %}
#### تلقائيا مع المكتبة:

#### CoinPayments: [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

{% hint style="info" %}
من الأفضل استخدام الطريقة ** `على الدخل` ** وليس الطريقة **` onION` **. 

لأن هذه الطريقة ** `onIncome` ** تغطي IPN بالكامل وتحل المشكلة [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

### Command `/onWalletCreate`

```javascript
//Bot.sendMessage(inspect(options));

let wallet = options.result.address;
Bot.sendMessage("Your permanent wallet address is:\n`" + wallet + "`")

// You can save wallet
//User.setProperty("wallet", wallet, "string");
```

### Command `/onIncome`

```javascript
// يمكن لأي شخص أن يركض
/onIncome command!

if(!options){
   // للأمن ، نحتاج إلى التحقق من أن هذا الأمر لا يعمل إلا من خلال lib
   // لا يمكن للمستخدم تشغيل الأمر مع الخيارات
   return
}

واسمحوا محفظة = options.address؛
دع العملة = options.currency؛
دع المبلغ = options.amount؛

دع fiat_amount = options.fiat_amount؛
دع fiat_currency = options.fiat_coin؛

اسمحوا رسوم = options.fee ؛

سمح 

// رؤية حقول أخرى من قبل
// Bot.sendMessage(inspect(options));

Bot.sendMessage(
   "*الدخل للمحفظة:*" +
   "\n`"+ wallet + "`" +
   "\n\n*Amount*:\n" +
amount + " " + currency + " (" + fiat_amount + " " + fiat_currency + ")" +
   "\n*Fee*: " + fee +
   "\n\nTXN: `" + txn_id + "`"
);

```



#### `command onError`

```javascript
// يمكنك فحص جميع المجالات:
Bot.sendMessage(inspect(options))
```





## استكشاف الأخطاء وإصلاحها وتصحيحها
* لا تستخدم نفس حساب CoinPayment لاستلام الأموال وتحويلها.
* اذهب إلى [page](https://www.coinpayments.net/index.php?cmd=acct_balances&action=deposits). يجب أن تحتوي هذه القائمة على سجل مع معاملة الدخل المكتملة\(s\)
* حاول إعادة إرسال IPN - راجع تصحيح الأخطاء
* تحقق من أن لديك
[set IPN secret](https://help.bots.business/libs/coinpayments#set-ipn-secret)

### IPN History

يمكنك عرض تاريخ IPN عن طريق الرابط [https://www.coinpayments.net/acct-ipn-history](https://www.coinpayments.net/acct-ipn-history)

![](../.gitbook/assets/image%20%2845%29.png)

كما يمكنك إعادة إرسال IPN عن طريق تحديد خانة الاختيار "إعادة إرسال" وزر "إعادة إرسال التحقق IPN\(s\)"

### Test methods

#### محفظة مؤقتة:

كما أنه من الممكن ** إجراء اختبار onPaymentCompleted ** الحدث. انه لامر جيد إذا كنت لا تريد إجراء اختبار الدفع.

```javascript
options = {
  onPaymentCompleted: "/onPaymentCompleted 0.75"
}

Libs.CoinPayments.callTestPaymentCompleted(options);
```

#### 

#### محفظة دائمة:

أيضا فمن الممكن ** إجراء اختبار ** callTestPermanentWalletIncome الحدث. انه لامر جيد إذا كنت لا تريد إجراء اختبار الدفع.

```javascript
options = {
  // onIPN: "/onPermanentWalletIPN",  // if you need IPN also
  onIncome: "/onIncome",
  
  // ليس الخيارات الضرورية
  // يمكنك تمرير المبلغ
  //amount: 0.5
  // txn_id: YOUR_TXN_ID
}

Libs.CoinPayments.callTestPermanentWalletIncome(options);
```



## الأمان

{% hint style="warning" %}
يوصى بشدة بالاهتمام بالسلامة عند استخدام هذه المكتبة.
{% endhint %}

** لا تستخدم الأسماء الافتراضية ** للأوامر الآمنة مثل
`/onIncome`, `/onPaymentCompleted`

يمكن لأي شخص تشغيل أي أمر بالأسماء. لذلك تحتاج إلى التحقق من ذلك الأمر الأمني ​​الذي تديره CoinPayment ليب فقط!

```javascript
if(!options){
   // للأمن ، نحتاج إلى التحقق من أن هذا الأمر لا يعمل إلا من خلال lib
   // لا يمكن للمستخدم تشغيل الأمر مع الخيارات
   return
}

// كودك الآمن
...
```

{% hint style="danger" %}
لا تستخدم أي libs غير الرسمية الآن.

* يمكن لأي ليب تشغيل الأمر مع الخيارات.
* يمكن لأي libs قراءة الخصائص \ (وقراءة مفاتيح API من lib الأخرى)

ليس لدينا طريقة لحماية هذا الآن. فقط لا
{% endhint %}



** قم بمنح الأذونات الضرورية حقًا لمفتاح Api. ** إذا لم تكن برامج bor بحاجة إلى "إنشاء \ _السحب" أو طرق أخرى - فأوقف تشغيلها. 

![Check all options what you need](../.gitbook/assets/image%20%2818%29.png)

قراءة المزيد عن الأمن [here](https://help.bots.business/scenarios-and-bjs/bjs-security)

