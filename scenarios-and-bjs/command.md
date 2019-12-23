# القيادة (الاوامر)



{% hint style="warning" %}
هذه الوظيفة ** مهملة **. الرجاء استخدام [this](https://help.bots.business/scenarios-and-bjs/send-http-request)
{% endhint %}

| وظيفة | وصف | مثال |
| :--- | :--- | :--- |
| `Command.setValue(name, value)` | set value to command | `Command.setValue("url", "http://example.com")` |



** يمكنك تمرير متغير إلى عنوان URL للنص السابق. ** هذا مفيد إذا كنت لا تريد إنشاء عنوان URL.

```text
->(<my_url>)
```

يجب تعيين المتغير `my_url` في البرنامج النصي السابق لهذا الأمر:

```javascript
Command.setValue("my_url", "http://example.com");

->(<my_url>)
```

هنا في السيناريو الأول ، يتم تعيين المتغير `my_url`. في السيناريو الثاني ، يتم تحميل الصفحة من [http://example.com](http://example.com/).

> يمكنك ضبط `my_url` في أي أمر آخر مع وظيفة
` Command.setValue`

