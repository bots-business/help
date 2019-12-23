---
الوصف: استخدم هذه المكتبة لتتبع الإحالة.
---

# RefferalLib

Demo bot: [https://telegram.me/DemoReferalTrackingBot?start=FromLibPage](https://telegram.me/DemoReferalTrackingBot?start=FromLibPage)

## ابدء

الوظيفة الأساسية هي ** المسار **.  تفضل أن نسميها على
/start:

`Libs.ReferralLib.currentUser.track(trackOptions);`

params `trackOptions` - هو كائن مع وظائف رد الاتصال من أجل:

<table>
  <thead>
    <tr>
      <th style="text-align:left">وظيفة رد الاتصال</th>
      <th style="text-align:left">الوصف</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">onTouchOwnLink()</td>
      <td style="text-align:left">اتصال المستخدم الرابط الخاص</td>
    </tr>
    <tr>
      <td style="text-align:left">onAlreadyAttracted()</td>
      <td style="text-align:left">المستخدم جذبت بالفعل</td>
    </tr>
    <tr>
      <td style="text-align:left">onAttracted()</td>
      <td style="text-align:left">تم جذب المستخدم عبر شانيل</td>
    </tr>
    <tr>
      <td style="text-align:left">onAtractedByUser(refUser)</td>
      <td style="text-align:left">
        <p>تم جذب المستخدم من قبل مستخدم refUser آخر - إنها بيانات مستخدم شائعة (الحقول:
           اللقب ، الاسم الأول ، إلخ)</p>
        <p></p>
        <p>لديك أيضا مجال الدردشة مع معرف الدردشة لهذا المستخدم.</p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
See @[DemoReferalTrackingBot](https://telegram.me/DemoReferalTrackingBot?start=FromLibPage) for details
{% endhint %}

## المهام



 ## الحصول على رابط الإحالة للمستخدم الحالي

`Libs.ReferralLib.currentUser.getRefLink(bot.name);` 

will generate link kind **http://t.me/botname?start=userUSER\_ID**

كما يمكنك تغيير البادئة.  على سبيل المثال إزالة "المستخدم"

`Libs.ReferralLib.currentUser.getRefLink(bot.name, "");` 

سوف تولد نوع الارتباط **http://t.me/botname?start=USER\_ID**

### 

### الحصول على جاذب للمستخدم الحالي

`Libs.ReferralLib.currentUser.attractedByUser()` 

إرجاع بيانات المستخدم \(with chatId\) 



### الحصول على قناة جذب للمستخدم الحالي

`Libs.ReferralLib.currentUser.attractedByChannel()` 

عودة قناة الذي تم جذب المستخدم الحالي



### الحصول على refList

`Libs.ReferralLib.currentUser.refList.get();` 

قائمة العودة مع المستخدمين جذبت



### مسح refList

`Libs.ReferralLib.currentUser.refList.clear();`

### 

### الحصول على أعلى قائمة الإحالة

`Libs.ReferralLib.topList.get(45)`

إرجاع أول 45 مستخدمًا حسب عدد الإحالات



### مسح أعلى قائمة الإحالة

`Libs.ReferralLib.topList.clear()`

** س: كيفية منح مكافأة للمستخدم عن صديق جذب؟ **

**Answer:**

يمكننا استخدام [ResourcesLib](https://help.bots.business/libs/resourceslib) for this.

on `/start`

```javascript
function onAttracted(refUser){
  // access to Bonus Res of refUser
  let refUserBonus = Libs.ResourcesLib.anotherUserRes("money", refUser.telegramid);
  refUserBonus.add(100);  // إضافة 100 مكافأة لصديق
}

Libs.ReferralLib.currentUser.track({
   onAtractedByUser: onAttracted
});
```



** س: كيفية إعطاء للإحالة 5 ٪ من إيداع المستخدم الإحالة؟ **

**Answer:**

1. تحتاج الإعداد [track](https://help.bots.business/libs/refferallib#getting-started) in first
2. يبدو أنك بحاجة إلى استخدام [ResourcesLib](https://help.bots.business/libs/resourceslib)
3. على رصيد مجموعة المستخدم:

```javascript
let res = Libs.ResourcesLib.userRes("money");
let referrer = Libs.ReferralLib.currentUser.AttractedByUser();

// if current user was attracted by referrer
if(referrer){
   let referrerRes = Libs.ResourcesLib.anotherUserRes(
       "money", referrer.telegramid);
   
   let amount = res.value * 0.05; // it is 5%
   referrerRes.takeFromAnother(res, amount);
}
```

{% hint style="info" %}
في هذا المثال ، نستخدم userRes.  كما أنه من الممكن استخدام chatRes.  نرى [ResourcesLib](https://help.bots.business/libs/resourceslib) for details
{% endhint %}

