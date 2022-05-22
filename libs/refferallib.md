---
description: Use this Lib for referral tracking.
---

# RefferalLib

**Demo bot:** [https://telegram.me/DemoReferalTrackingBot](https://telegram.me/DemoReferalTrackingBot)

## Getting started

Basic function is **track**. Prefer to call it on /start:

`Libs.ReferralLib.track(trackOptions);`

params `trackOptions` - it is object with callback functions for:

| **Callback function**  | **Description**                                                                                           |
| ---------------------- | --------------------------------------------------------------------------------------------------------- |
| `onTouchOwnLink()`     | user touch own ref link                                                                                   |
| `onAlreadyAttracted()` | user already attracted                                                                                    |
| `onAttracted(byUser)`  | user was attracted by other user byUser - it is common user data (fields: nickname, first\_name and etc)  |

{% hint style="info" %}
See [@DemoReferalTrackingBot](https://telegram.me/DemoReferalTrackingBot?start=FromLibPage) for details
{% endhint %}

## Functions



## Get Referral link for current user

`Libs.ReferralLib.getLink();`&#x20;

will generate link kind **http://t.me/botname?start=userUSER\_ID**

Also you can pass other bot name. For example - it is link for current bot:

`Libs.ReferralLib.getRefLink(bot.name);`&#x20;

will generate link kind **http://t.me/botname?start=userUSER\_ID**

****

#### Do you want to change link prefix?

For example you want link without "user" prefix. Use this code once:

`Libs.ReferralLib.setLinkPrefix("")`

then code:

`Libs.ReferralLib.getLink()`

will generate link kind:

**http://t.me/botname?start=USER\_ID**



###

### Get attractor for current user

`Libs.ReferralLib.getAttractedBy()`&#x20;

return attractor user data





### Get refList

`Libs.ReferralLib.getRefList();`&#x20;

return [list](../bjs/lists/) with attracted users.



Or get for Ref List for another user:

`Libs.ReferralLib.getRefList(another_user_id);`

{% hint style="info" %}
This method return users [list](../bjs/lists/). You can [paginate](../bjs/lists/#paginating) it, [sort](../bjs/lists/#ordering), [recount](../bjs/lists/#recount-list) it and etc.
{% endhint %}

then code  for `/reflist` can be:

```javascript
let refList = Libs.ReferralLib.getRefList();

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
  users_rows = users_rows + "\n👤 " + Libs.commonLib.getLinkFor( users[ind] )
}

let msg =
  "*Total users:* " +
  Libs.ReferralLib.getRefCount() +
  "\n _the first user was tracked:_ \n" +
  "   _" +
  refList.created_at +
  "_" +
  "\n----" +
  users_rows
  
Bot.sendMessage(msg);
```



### Get refferals count

`Libs.ReferralLib.getRefCount()`

or for another user:

`Libs.ReferralLib.getRefCount(another_user_id)`

``

### Get Top Refferal List

`Libs.ReferralLib.getTopList()`

```javascript
// It is just List
// you can order, paginate it!
// https://help.bots.business/bjs/lists#getting-data 
let list = Libs.ReferralLib.getTopList();

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
    Libs.commonLib.getLinkFor(prop.user) + ": 👨" +
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
  let refUserBonus = Libs.ResourcesLib.anotherUserRes("money", refUser.telegramid);
  refUserBonus.add(100);  // add 100 bonus for friend
}

Libs.ReferralLib.track({
   onAttracted: onAttracted
});
```



**Q: how to give to referrer 5% of referral user deposit?**

**Answer:**

1. You need setup [track](https://help.bots.business/libs/refferallib#getting-started) in first
2. Seems you need use [ResourcesLib](https://help.bots.business/libs/resourceslib)
3. On user set balance:

```javascript
let res = Libs.ResourcesLib.userRes("money");
let referrer = Libs.ReferralLib.getAttractedBy();

// if current user was attracted by referrer
if(referrer){
   let referrerRes = Libs.ResourcesLib.anotherUserRes(
       "money", referrer.telegramid);
   
   let amount = res.value * 0.05; // it is 5%
   referrerRes.takeFromAnother(res, amount);
}
```

{% hint style="info" %}
In this example we use userRes. Also it is possible use chatRes. See [ResourcesLib](https://help.bots.business/libs/resourceslib) for details
{% endhint %}
