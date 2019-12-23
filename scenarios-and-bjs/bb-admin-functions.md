# BB Admin functions

## BBAdmin.attractUser

| وظيفة | الوصف |
| :--- | :--- |
| `BBAdmin.attractUser(خيارات)` | جذب مستخدم جديد. |

يجذب مالك Bot مستخدمًا جديدًا بالبريد الإلكتروني "test@example.com" إلى Bots.Business.  يجب ان يكون البريد الاكتروني صحيح.

* سيتم تلقي المستخدم البريد الإلكتروني مع كلمة المرور والمعلومات للبدء.
 * مالك بوت يمكن أن يرى مستخدم جديد في التطبيق -> الحساب -> قائمة المستخدمين الجذابة
 * صاحب بوت لديه مكافآت لكل مستخدم جذبت مع خطة مدفوعة

{% hint style="info" %}
هذه الوظيفة تعمل فقط للمستخدمين الجدد.  للمستخدمين القدامى - لا يوجد أي جاذبية ولا بريد إلكتروني.
{% endhint %}

```javascript
BBAdmin.attractUser(
  { email: 'test@example.com'}
)
```

كما أنه من الممكن تمرير
owner\_id \(إذا لم يتم تعريفه - يستخدم معرف مالك bot\)

```javascript
BBAdmin.attractUser(
  { 
    email: 'test@example.com',
    owner_id: user_id // 
  }
)
```

## BBAdmin.installBot

| وظيفة | الوصف |
| :--- | :--- |
| BBAdmin.installBot \ (خيارات \) | تثبيت بوت للمستخدم الآخر |

يمكنك تثبيت نسخة لك موجودة بوت للمستخدم مع البريد الإلكتروني.

{% hint style="info" %}
إذا كان المستخدم مستخدمًا جديدًا - فسيتم جذبهم كإحالة لك.
{% endhint %}

{% hint style="warning" %}
يمكنك تثبيت الروبوت الخاص بك فقط للمستخدمين الآخرين
{% endhint %}

{% hint style="info" %}
يمكنك تمرير خصائص bot لبوت جديد
{% endhint %}

```javascript
BBAdmin.installBot(
  { email: 'test@example.com',
    bot_id: 15025, // رؤية معرف الروبوت في التطبيق -> السير -> بوت
    // يمكنك تمرير الخصائص إلى bot:
    // bot_properties: [{ name: 'test', value:'hello world', type:'string' }]
  }
)
```

{% hint style="info" %}
يمكنك استنساخ الروبوت الخاص بك مع هذه الوظيفة
{% endhint %}

