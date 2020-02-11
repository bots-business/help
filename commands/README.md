---
الوصف: ما هو "bot command"؟
---

# Commands

Command:
إنه نص من المستخدم.  يمكن أن ترسل بوت الجواب عن القيادة أو القيام بشيء ما.  عادة ما تبدأ الأمر مع
`"/"`, e.g. `/hello`.
لكن هذا ليس مطلوبًا دائمًا.

{% hint style="warning" %}
`/start` and `/START` -
انها ليست نفس الأوامر.  القيادة حساسة لحالة الأحرف
{% endhint %}

### كيفية تنفيذ الأوامر مع أي نص من المستخدم؟
\(Master command\)

مجرد استخدام `*` في اسم الأمر.

انظر 
[more](https://help.bots.business/scenarios-and-bjs/always-running-commands)



### مجالات القيادة

القيادة يمكن ان تكون:

<table>
  <thead>
    <tr>
      <th style="text-align:left">حقل</th>
      <th style="text-align:left">وصف</th>
      <th style="text-align:left">مثال</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>الامر</code>
      </td>
      <td style="text-align:left">استخدام لنداء القيادة</td>
      <td style="text-align:left">
        <p><code>"/start"</code>, <code>&quot;/run&quot;</code>لاي نص استخدم<code>"*"</code>
        </p>
        <p><b>حساسية الموضوع</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>help</code>
      </td>
      <td style="text-align:left">وصف الامر</td>
      <td style="text-align:left"><code>"مرحبا بكم في RentBot. انظر /help أو /order الآن"</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>الجواب</code>
      </td>
      <td style="text-align:left">نص الجواب</td>
      <td style="text-align:left"><code>مرحبا تحتاج الى register/ قبل المتابعة</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>aliases</code>
      </td>
      <td style="text-align:left">استخدام لدعوة القيادة البديلة</td>
      <td style="text-align:left"><code>/welcome, /hello, ?, help</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>لوحة المفاتيح</code>
      </td>
      <td style="text-align:left">إرسال لوحة المفاتيح للمستخدم على دعوة القيادة</td>
      <td style="text-align:left"><code>&quot;الامر, حول&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>سيناريوهات</code>
      </td>
      <td style="text-align:left"><code>BJS</code> رمز للتنفيذ</td>
      <td style="text-align:left"><code>2+2</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>group</code>
      </td>
      <td style="text-align:left">الامر متاح فقط لمستخدمي هذة المجموعة</td>
      <td style="text-align:left"><code>guests</code>, <code>clients</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>need_reply</code>
      </td>
      <td style="text-align:left">الامر انتظر الجواب من المستخدم.  يمكن أن تكون صحيحة أو خاطئة أو فارغة. <code>true</code> BJS  تنفيذ التعليمات البرمجية بعد اجابة المستخدم</td>
      <td style="text-align:left"><code>true</code>, <code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>auto_retry</code>
      </td>
      <td style="text-align:left">يمكن تشغيل الأمر مع فاصل زمني في ثوان</td>
      <td style="text-align:left"><code>600</code> - كرر مرة واحدة في 10 دقيقة</td>
    </tr>
  </tbody>
</table>كيفية إنشاء وتحرير الأوامر؟###

**يمكنك تعديل الأمر مباشرة من التطبيق.**

![Screen from App for command creation](../.gitbook/assets/image%20%2812%29.png)

### Commands importing

اصنع كل الاوامر باستخدام [Google Table. ](https://help.bots.business/create-bot-from-google-table)

كما يمكنك نسخ جدول القالب من 
[http://bit.ly/bb\_table\_template](http://bit.ly/bb_table_template)
 في الجدول الخاص بك. 

هذا خيار مثالي: كل شيء بسيط للغاية.  اذهب إلى
`Main menu > File > Make a copy`.
ستحتاج إلى ورقة منفصلة للأوامر التي ستحتوي على الأوامر.  ثم يمكنك إضافة أوامر في الصفوف والقيام باستيراد ملف CSV من التطبيق.




### 