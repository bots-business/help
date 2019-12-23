# Admin Panel

\*\*\*\*

** يمكنك إنشاء لوحة إدارة مخصصة. **

* جعل حقول البيانات المخصصة: الرقمية ، النص ، مربع ، كلمة المرور
 * حقول البيانات ستكون متاحة في BJS
 * يمكن إنشاء لوحات المشرف عبر BJS
 * بوت تشغيل قيادة مخصصة في مجال الادخار
 * يدعم الألواح الفاصلة مع الألقاب وحقول الاختلافات

**فوائد:**

* صنع خيارات لحفظ أي مفاتيح api ، بيانات آمنة وغير آمنة
 * يمكن تشغيل أي منطق BJS من لوحة.  على سبيل المثال: سيكون من الممكن إنشاء حقل نص مع زر "إرسال هذه الرسالة إلى جميع الدردشات" من التطبيق.
 * جعل أي إحصائية سريعة والمعلومات.  بوت لوحات القيادة وغيرها

![](../.gitbook/assets/image%20%2829%29.png)

## اساليب

### تحديد لوحة الإدارة الجديدة

لوحة الإدارة - هذا هو مزيج من العديد من اللوحات.  كل لوحة لها
title, icon, description
وحقل واحد أو أكثر:

لإضافة لوحة:

`AdminPanel.setPanel({ panel_name: PANEL_NAME, data: PANEL_OPTIONS });`

PANEL\_OPTIONS - إنه خيار JSON لهذه اللوحة

**مثال**

![](../.gitbook/assets/image%20%2850%29.png)

```javascript
var panel = {
  // Pabel عنوان
  title: "معلومات المسؤول",
  description: "يرجى ملء هنا معرف المسؤول الخاص بك",
  // مؤشر الطلب
  index: 0,
  icon: "key",
  // حفظ عنوان الزر - الافتراضي "حفظ"
  button_title: "SAVE" ،
  // أمر يسمى على الحفظ
  // ليس من الضروري
  /* on_saving:{
     command: "/on-saving",
     // إذا كنت بحاجة إلى المستخدم
     user_id: user_id // Get it via Bot.sendMessage(user.id)
  },
  */
  
  // الحقول لهذا الفريق
  // هنا 1 حقل فقط
  fields: [
    {
      name: "ADMIN_ID",
      title: "Admin ID",
      description: "you can get your admin_id with BJS Bot.sendMessage(user.id)",
      type: "string",
      placeholder: "your admin id",
      // القيمة: 100
    }
    // حقول أخرى هنا
    // إذا لزم الأمر
    // ...
  ]
}

AdminPanel.setPanel({
  panel_name: "AdminInfo",
  data: panel
  // اجبر: صحيح
// default false - احفظ قيم الحقول
});
```

#### فرض

 الافتراضي غير صحيح.  يتم الاحتفاظ بجميع القيم القديمة للحقول.

 إذا كان هذا صحيحًا - يتم إعادة تعيين جميع القيم القديمة للحقول.

 #### مجالات

 إنه مجموعة من الحقول.  لوحة واحدة يمكن أن يكون لها العديد من المجالات.  بل هو أيضا لوحة ممكنة دون أي مجال.

الحقول يمكن أن تكون
name, value, title, description, type, placeholder and icon

#### Field type

| اكتب | الوصف |
| :--- | :--- |
| مربع الاختيار | إدخال النص |
| عدد صحيح | إدخال النص |
| تعويم | إدخال النص |
| سلسلة | إدخال النص |
| كلمة المرور | إدخال كلمة المرور |
| نص | حقل النص |

### 

### الحصول على قيمة الحقل من لوحة

{% hint style="success" %}
استخدم هذه الطريقة للحصول على قيمة واحدة من اللوحة
{% endhint %}

```javascript
var admin_id = AdminPanel.getFieldValue({
  panel_name: "AdminInfo", // اسم اللوحة
  field_name: "ADMIN_ID" // اسم الحقل
})

Bot.sendMessage(admin_id)
```

### الحصول على جميع قيم الحقول من لوحة

{% hint style="success" %}
استخدم هذه الطريقة للحصول على عدة
/جميع القيم من لوحة
{% endhint %}

```javascript
var values = AdminPanel.getPanelValues("AdminInfo");
Bot.inspect(values);
// سيكون مثل:
// { ADMIN_ID: 100 }
```

###  

### الحصول على بيانات اللوحة

```javascript
var panel = AdminPanel.getPanel("AdminInfo")
Bot.inspect(panel);

// يمكن تعديل لوحة
// panel.fields[0].value = 1000
// panel.fields[0].tite = "my admin id"
// AdminPanel.setPanel("AdminInfo", panel);
```

### 

### الحصول على بيانات حقل اللوحة

```javascript
var panel_field = AdminPanel.getPanelField({
  panel_name: "AdminInfo", // اسم اللوحة
  field_name: "ADMIN_ID" // اسم الحقل
})

Bot.inspect(panel_field);
```

### الرموز

يمكنك استخدام جميع الرموز من 
[https://ionicons.com](https://ionicons.com/)



## الممارسات الجيدة

{% hint style="success" %}
استخدم لوحات الإدارة لإنشاء **تكوين **
{% endhint %}

تحديد لوحات الإدارة في
`/config`
قيادة مع طريقة
`AdminPanel.setPanel`.

ثم استخدام
`AdminPanel.getPanelValue`
طريقة للحصول على قيمة أي مجال



{% hint style="success" %}
استخدم لوحات الإدارة لإنشاء لوحة **معلومات **
{% endhint %}

لوحة بدون حقول يمكن نشر أي معلومات.  استخدم هذا.



{% hint style="success" %}
استخدم لوحات المشرف لإنشاء ردود فعل المشرف
{% endhint %}

يمكنك تشغيل أي أمر bot في توفير اللوحة.  انه لامر جيد لتنفيذ أي تنفيذ الأوامر المشرف.  استعمل
`on_saving`: 

```javascript
var panel = {
  // Pabel عنوان
  title: "استدعاء قيادة آمنة",
  description: "إنه أمر آمن",
  // مؤشر الطلب
  index: 0,
  icon: "key",
  // حفظ عنوان الزر - الافتراضي "حفظ"
  button_title: "RUN",
  // أمر يسمى على الحفظ
  // ليس من الضروري
  on_saving: {
     command: "/secure-command",
     // إذا كنت بحاجة إلى المستخدم
     user_id: user_id // Get it via Bot.sendMessage(user.id)
  }
}

AdminPanel.setPanel("SecureCommand", panel);
```

