---
description: Use this Lib for referral tracking.
---

# RefferalLib

**Demo bot:** [https://telegram.me/DemoReferalTrackingBot](https://telegram.me/DemoReferalTrackingBot)

{% hint style="info" %}
RefferalLib is core Lib now - installation is not needed!
{% endhint %}

## Getting started

Basic function is **track**. Prefer to call it on /start:

`RefLib.track(trackOptions);`

params `trackOptions` - it is object with callback functions for:

| Attribute              | **Description**                                                                                                                                                                                                                                                                                       |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `onTouchOwnLink()`     | user touch own ref link                                                                                                                                                                                                                                                                               |
| `onAlreadyAttracted()` | user already attracted                                                                                                                                                                                                                                                                                |
| `onAttracted(byUser)`  | user was attracted by other user byUser - it is common user data (fields: nickname, first\_name and etc)                                                                                                                                                                                              |
| `linkPrefix`           | <p>Prefix for link. By default it is "user":<br>https://t.me/botName?start=<strong>user</strong>ID<br><br>You can change linkPrefix in any time but all old links will be broken!<br><br>Please check all your <a href="../deep-linking-pass-any-params-on-bot-starting.md">deep link</a> params!</p> |

{% hint style="info" %}
See [@DemoReferalTrackingBot](https://telegram.me/DemoReferalTrackingBot?start=FromLibPage) for details (Available in the Store)
{% endhint %}

### Example

```javascript
// this function will be executed if user poress own ref link
function onTouchOwnLink(){
   Bot.sendMessage("It is your ref link!")
}

// user can restart bot by ref link again
function onAlreadyAttracted(){
   Bot.sendMessage("You already joined")
}

// it is new user. He start bot via ref link
function onAttracted(byUser){
   Bot.sendMessage("Thank you for joining!" +
                    "Your friend TG id is: " + byUser.telegramid)
   // you can add bonus here...
}

RefLib.track({
   onTouchOwnLink: onTouchOwnLink,
   onAlreadyAttracted: onAlreadyAttracted,
   onAttracted: onAttracted,
   linkPrefix: "user", // you can use "", "r" and etc
   // you can pass debug for external debug info
   //debug: true
});
```

## Functions

### Get Referral link for current user

`RefLib.getLink();`&#x20;

will generate link kind **http://t.me/botname?start=userUSER\_ID**



Also you can pass other bot name. For example - it is link for current bot:

`RefLib.getLink(bot.name);`&#x20;

will generate link kind http://t.me/**botname**?start=userUSER\_ID



It is possible to change link prefix:

`RefLib.getLink(bot.name, "r");`&#x20;

will generate link kind http://t.me/botname?start=**r**USER\_ID





### Get attractor for current user

`RefLib.getAttractedBy()`&#x20;

return attractor user data



### Remove ref data for

It is test method. You can run it and check ref link again like new user.

`RefLib.clearRef()`



### Get refList

`RefLib.getRefList();`&#x20;

return [list](../bjs/lists/) with attracted users.



Or get for Ref List for another user:

`RefLib.getRefList(another_user_id);`

{% hint style="info" %}
This method return users [list](../bjs/lists/). You can [paginate](../bjs/lists/#paginating) it, [sort](../bjs/lists/#ordering), [recount](../bjs/lists/#recount-list) it and etc.
{% endhint %}

then code  for `/reflist` can be:

```javascript
let refList = RefLib.getRefList();

if (!refList.exist) {
  Bot.sendMessage("No any affiliated users")
  return
}

let users_rows = ""

// only 100 first users here
// for other users you need use pagination:
// https://help.bots.business/bjs/lists#paginating
let users = refList.getUsers();

for (var ind in users) {
  users_rows = users_rows + "\n👤 " + CommonLib.getLinkFor( users[ind] )
}

let msg =
  "*Total users:* " +
  RefLib.getRefCount() +
  "\n _the first user was tracked:_ \n" +
  "   _" +
  refList.created_at +
  "_" +
  "\n----" +
  users_rows
  
Bot.sendMessage(msg);
```



### Get refferals count

`RefLib.getRefCount()`

or for another user:

`RefLib.getRefCount(another_user_id)`





### Get Top Refferal List

{% hint style="danger" %}
Unfortunately we had to remove sorting in sheets due to a performance issue.

This functionality does not yet work.
{% endhint %}

~~`RefLib.getTopList()`~~

```javascript
// It is just List
// you can order, paginate it!
// https://help.bots.business/bjs/lists#getting-data 
let list = RefLib.getTopList();

list.order_by = "integer_value";
// olso it is possible get newest members:
list.order_ascending = false;

var items = list.get();
//Bot.inspect(items);

var msg = 'Top list: ';
var prop;
for(var ind in items){
  prop = items[ind]
  msg = msg + "\n" +
    String( parseInt(ind) + 1 ) + ". " + 
    CommonLib.getLinkFor(prop.user) + ": 👨" +
    String(prop.value)
}

Bot.sendMessage(msg);
```

## How to

**Q: How to give bonus to user for attracted friend?**

**Answer:**

We can use [ResourcesLib](https://help.bots.business/libs/resourceslib) for this.

on `/start`

```javascript
function onAttracted(refUser){
  // access to Bonus Res of refUser
  let refUserBonus = ResLib.anotherUserRes("money", refUser.telegramid);
  refUserBonus.add(100);  // add 100 bonus for friend
}

RefLib.track({
   onAttracted: onAttracted
});
```



**Q: how to give to referrer 5% of referral user deposit?**

**Answer:**

1. You need setup [track](https://help.bots.business/libs/refferallib#getting-started) in first
2. Seems you need use [ResLib](https://help.bots.business/libs/resourceslib)
3. On user set balance:

```javascript
let res = ResLib.userRes("money");
let referrer = RefLib.getAttractedBy();

// if current user was attracted by referrer
if(referrer){
   let referrerRes = ResLib.anotherUserRes(
       "money", referrer.telegramid);
   
   let amount = res.value * 0.05; // it is 5%
   referrerRes.takeFromAnother(res, amount);
}
```

{% hint style="info" %}
In this example we use userRes. Also it is possible use chatRes. See [ResourcesLib](https://help.bots.business/libs/resourceslib) for details
{% endhint %}
