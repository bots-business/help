# Answer

{% hint style="info" %}
Answer - إنها رسالة نصية بسيطة من الروبوت على تنفيذ الأوامر.
{% endhint %}

![Answer can be modified on command editing ](../.gitbook/assets/image%20%2840%29.png)

### كيفية تنسيق أوامر الإجابة؟

استخدم نصوص Markdown:

```text
\n - new line (يسمح بنص متعدد الخطوط أيضا)
*bold text*
_ italics _
[text](URL)
`inline fixed-width code`
```text
pre-formatted fixed-width code block
```



### كيفية إدراج رابط للإجابة؟

```text
[link text](url)
```

مثال:

```text
[Google](http://google.com)
```



### كيفية إدراج صورة للإجابة؟

```text
[picture title](url)
```

مثال:

```text
[Dog](http://example.com/images/dog.jpg)
```

كما يمكنك إرسال ملف الصورة مباشرة مع BJS.



### كيفية إدراج أمر آخر في الإجابة؟

مجرد استخدام هذا النص `command/`

مثال:

```text
Hello! You can now /register or read /help
```

### كيفية إدراج رابط إلى أمر آخر على الجواب؟

يمكن ذلك مع  "/"
غير ذلك
, e.g.: `/help`, للأسف, لا.

