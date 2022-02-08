# GoogleTableSync

You can post, update and read data from GoogleSpreadSheet with this lib.

![](<../.gitbook/assets/image (95) (1).png>)

### Setup

1\. Please see setup for [GoogleApp lib](googleapp.md). This lib requires GoogleApp lib.

2\. Create blank Google table. You don't need any headers. Headers will be added automatically.

3\. Also you need to run code too:

`Libs.GoogleApp.setUrl("https://script.google.com/*************/exec");`

{% hint style="info" %}
If you have any problems with this lib - use help from [GoogleApp lib](googleapp.md).
{% endhint %}

### Demo bot

{% embed url="https://telegram.me/BBGoogleSpreadsheetBot" %}

### Write or update data

{% hint style="success" %}
Use `index` for magic!

With index key exist row will be updated!
{% endhint %}

You need tableID. You can get it from table url. It is selected here:

![](<../.gitbook/assets/image (95).png>)

```javascript
Bot.sendMessage("Saving...");

var syncOptions = {
  tableID: "1_NldI2**********ank1B9c",
  sheetName: "Users",
  // this column will be used as index for updates or reading
  index: "id",
  // store data
  datas: [],
  // this command will be runned after sync
  onRun: "/onSync",

  // for debug. Comment this lines
  // email: "hello@bots.business",
  // debug: true
}

user.balance = User.getProperty("balance");

syncOptions.datas[0] = user;

// you can add more records with index 
// syncOptions.datas[1] and etc 

Libs.GoogleTableSync.sync(syncOptions);
```

{% hint style="success" %}
You can store any data like users, chats, products, resources and etc
{% endhint %}

```
Bot.sendMessage("Saving...");

var syncOptions = {
  tableID: "1_NldI2**********ank1B9c",
  sheetName: "Orders",
  // this column will be used as index for updates or reading
  index: "orderId",
  datas: [],
  // this command will be runned after sync
  onRun: "/onSync"
}

var order = {
   orderId: 10,
   title: "Order - " + String(user.id),
   amount: 15
}

syncOptions.datas.push(order);
Libs.GoogleTableSync.sync(syncOptions);
```

### Read data

```javascript
Bot.sendMessage("Reading...");

var syncOptions = {
  tableID: "1_NldI2**********ank1B9c",
  sheetName: "Users",
  // this column will be used as index for updates or reading
  index: "id",
  // reading data
  datas: [],
  // this command will be runned after sync
  onRun: "/onSync",

  // for debug. Comment this lines
  // email: "hello@bots.business",
  // debug: true
}

syncOptions.datas[0] = user;
// also you can use something like:
// syncOptions.datas[0] = { id: any_user_id }

// you can add more records with index 
// syncOptions.datas[1] and etc 


Libs.GoogleTableSync.read(syncOptions)
```



### Command /onRun

`Bot.inspect(options)`
