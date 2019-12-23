# GoogleSpreadSheet

![](../.gitbook/assets/image.png)

يمكنك نشر وقراءة البيانات من Google SpreadSheet مع هذه المكتبة.

## بوت التجريبي

{% embed url="https://telegram.me/BBGoogleSpreadsheetBot" %}



## بدء

** 1 **. .صنع جدول بيانات جديد.

![](../.gitbook/assets/image%20%2826%29.png)

تحتاج بالتأكيد رأس الجدول.  أصنعها.

** 2. ** من ورقة Google الخاصة بك ، من قائمة _ أدوات_ ، حدد _Script Editor_

** 3 ** لصق البرنامج النصي من [above](https://gist.github.com/bots-business/b627418423a2c5df3b4ed329181077f0) into _محرر رمز البرنامج النصي واضغط على _Save._

![](../.gitbook/assets/image%20%281%29.png)

** 4.. ** من قائمة _Publish_ ، حدد _Deploy كتطبيق ويب ... _ 
  
** 5.. ** اختر تنفيذ التطبيق بنفسك ، واسمح لـ _Anyone ، حتى المجهول_ بتنفيذ البرنامج النصي.  \ (ملاحظة ، بناءً على مثيل تطبيقات Google ، قد لا يكون هذا الخيار متاحًا. ستحتاج إلى الاتصال بمشرف تطبيقات Google ، أو استخدام حساب Gmail آخر. \) انقر الآن على _Deploy_.  قد تتم مطالبتك بمراجعة الأذونات الآن.

! [يظهر الإطار المنبثق بمجرد وجودك في محرر Google Script ، واخترت "نشر" ، و "نشر كتطبيق ويب ...". يعرض رسالة تقول "تم نشر هذا المشروع الآن 
[app](https://static1.squarespace.com/static/51814c87e4b0c1fda9c1fc50/t/5ab15222758d468109439ada/1521570432794/2-google-sheets-script-deploy.png?format=500w)

** 6. ** سيكون عنوان URL الذي تحصل عليه هو webhook التي تحتاج إلى استخدامها في هذا Lib.  يمكنك اختبار هذا webhook في متصفحك أولاً عن طريق لصقه.  لاحظ أنه وفقًا لمثيل تطبيقات Google ، قد تحتاج إلى ضبط عنوان URL لجعله يعمل.

**`/setup`**

```javascript
// استبدل عنوان URL الخاص بك ، الذي تم الحصول عليه في الخطوة 6
Libs.GoogleSpreadSheet.setUrl("https://script.google.com/macros/*******");
```



## باستخدام

 ### أضف صفًا جديدًا إلى جدول البيانات.

command /add

```javascript
let newRow = {
  'Country': 'Italy',
  'Age': '25',
  'Do you like Bots.Business?': 'YES'
}

let prms = {
  sheetName: "Users",  // اسم الورقة
  row: newRow,
  onSuccess: "onSuccess",  // هذا الأمر سوف يتم تنفيذها بنجاح
  onError: "onError"       // سيتم تنفيذ هذا الأمر على خطأ

}

Libs.GoogleSpreadSheet.addRow(prms)
```

### `onSucess` command

```javascript
// يمكنك فحص الخيارات:
// Bot.sendMessage(inspect(options));

let rowIndex = options.rowIndex;
User.setProperty("rowIndex", rowIndex, "integer"); // يمكنك ضبط فهرس الصفوف على الخيارات

Bot.sendMessage("Posted at row: " + rowIndex + 
    "\nInserted values: " + options.inserted);
```

### `onError` command

```javascript
Bot.sendMessage(inspect(options));
```

### Edit row.

```javascript
let newRow = {
  'Country': 'France',
  'Age': '18',
  'Do you like Bots.Business?': 'YES'
}

let prms = {
  sheetName: "Users",  // اسم الورقة
  row: newRow,
  onSuccess: "onSuccess",  // سيتم تنفيذ هذا الأمر بنجاح
  onError: "onError"       // سيتم تنفيذ هذا الأمر على خطأ
}


prms.rowIndex = 3;  // مؤشر الصف للتحرير
Libs.GoogleSpreadSheet.editRow(prms);

```

### الحصول على البيانات من جدول البيانات
 يمكن تغيير الصف في جدول البيانات.

```javascript
var rowIndex = 3; // مؤشر الصف للقراءة

Libs.GoogleSpreadSheet.getRow({
  sheetName: "Users",
  rowIndex: rowIndex,
  onSuccess: "onSuccessRead",
  onError: "onError"
})
```

### `onSuccessRead` command

```javascript
Bot.sendMessage(inspect(options.row));
```

