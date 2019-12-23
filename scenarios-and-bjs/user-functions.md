# وظائف المستخدم

<table>
  <thead>
    <tr>
      <th style="text-align:left">وظيفة</th>
      <th style="text-align:left">الوصف</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>User.setProperty(name, value, type)</code>
      </td>
      <td style="text-align:left">
        <p>تعيين خاصية مع اسم للمستخدم.  الاسم حساس لحالة الأحرف.</p>
        <p></p>
        <p><code>User.setProperty ("المدينة" ، "لندن" ، "السلسلة")</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.getProperty (الاسم)</code>
      </td>
      <td style="text-align:left">
        <p>قراءة الممتلكات مع الاسم.  الاسم حساس لحالة الأحرف.</p>
        <p></p>
        <p><code>User.getProperty ( "المدينة")</code>
          <br />
          <br />يمكن الحصول على خاصية ذات قيمة افتراضية لخاصية غير موجودة:</p>
        <p><code>User.getProperty ("المدينة" ، "لندن")</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.addToGroup(group_name)</code>
      </td>
      <td style="text-align:left">
        <p>إضافة مستخدم إلى مجموعة مع group_name</p>
        <p></p>
        <p><code>User.addToGroup(&quot;guests&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.getGroup()</code>
      </td>
      <td style="text-align:left">Get current user&apos;s group</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.removeGroup()</code>
      </td>
      <td style="text-align:left">إزالة المستخدم من المجموعة الحالية</td>
    </tr>
  </tbody>
</table>:الوصول إلى الممتلكات ** في الإجابة**

> يمكنك أيضًا استخدام الخصائص في إجابة الأمر.  على سبيل المثال ، يمكنك القيام بذلك مع
/ hello command:`Hello, <UserRole>!`

in BJS:

> ويمكنك استخدامها في
`Bot.sendMessage("Hello, <UserRole>")`

