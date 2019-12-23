# استيراد التلقائي على دفع جيت

يمكنك جعل نشر الروبوت التلقائي على بوابة دفع. 

هذا ممكن مع 
[Webhooks](https://help.bots.business/libs/webhooks-lib). امكانية تثبيت لهذا ليب.

## Setup

command `/setupGit`

```javascript
url = Libs.Webhooks.getUrlFor(
   { command: "onGitPush", user_id: user.id }
)

Bot.sendMessage(url);
```

نفذ
 `/setupGit` 
انسخ عنوان url وانتقل إلى Github.com> مستودعك -> الإعدادات -> Webhooks. اضغط على زر "أضف webhook"

تم نسخ عنوان url السابق كعنوان URL للحمولة

![](../.gitbook/assets/image%20%2838%29.png)

اجعل مثل هذا:

![](../.gitbook/assets/image%20%2844%29.png)

انتقل إلى التطبيق - إنشاء القيادة
`onGitPush`

```javascript
Bot.sendMessage("Start code importing...");

// Bot.exportGit من الممكن أيضا
Bot.importGit({
  branch: "master", // إنه فرع رئيسي
  نجاح: "onGitImportCompleted"
})
```

أمر `onGitImportCompleted`

وضعت للتو للإجابة: "اكتمال استيراد بوابة"

{% hint style="warning" %}
الأوامر 
`onGitPush و onGitImportCompleted` 
يجب تكون في مستودع أيضا. لأنه سيتم حذف جميع الأوامر عند استيراد بوابة
{% endhint %}

{% hint style="danger" %}
حماية onGitPush الأمر إذا كنت بحاجة إلى هذا. يمكن لأي شخص تشغيله. 
{% endhint %}





