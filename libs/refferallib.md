---
description: Use this Lib for referral tracking.
---

# RefferalLib

Demo bot: [https://telegram.me/DemoReferalTrackingBot?start=FromLibPage](https://telegram.me/DemoReferalTrackingBot?start=FromLibPage)

## Getting started

Basic function is **track**. Prefer to call it on /start:

`Libs.ReferralLib.currentUser.track(trackOptions);`

params `trackOptions` - it is object with callback functions for:

<table>
  <thead>
    <tr>
      <th style="text-align:left">callback function</th>
      <th style="text-align:left">description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">onTouchOwnLink()</td>
      <td style="text-align:left">user touch own link</td>
    </tr>
    <tr>
      <td style="text-align:left">onAlreadyAttracted()</td>
      <td style="text-align:left">user already attracted</td>
    </tr>
    <tr>
      <td style="text-align:left">onAttracted()</td>
      <td style="text-align:left">user was attracted via chanell</td>
    </tr>
    <tr>
      <td style="text-align:left">onAtractedByUser(refUser)</td>
      <td style="text-align:left">
        <p>user was attracted by other user refUser - it is common user data (fields:
          nickname, first_name and etc)</p>
        <p></p>
        <p>Also have field chatId with chat id for this user.</p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
See @[DemoReferalTrackingBot](https://telegram.me/DemoReferalTrackingBot?start=FromLibPage) for details
{% endhint %}

## Functions



## Get Referral link for current user

`Libs.ReferralLib.currentUser.getRefLink(bot.name);` 

will generate link kind **http://t.me/botname?start=userUSER\_ID**

Also you can change prefix. For example remove "user"

`Libs.ReferralLib.currentUser.getRefLink(bot.name, "");` 

will generate link kind **http://t.me/botname?start=USER\_ID**

### 

### Get attractor for current user

`Libs.ReferralLib.currentUser.attractedByUser()` 

return user data \(with chatId\) 



### Get attracted channel for current user

`Libs.ReferralLib.currentUser.attractedByChannel()` 

return Channel wich current user was attracted



### Get refList

`Libs.ReferralLib.currentUser.refList.get();` 

return list with attracted users



### Clear refList

`Libs.ReferralLib.currentUser.refList.clear();`

### 

### Get Top Refferal List

`Libs.ReferralLib.topList.get(45)`

return first 45 users ordering by referrals count



### Clear Top Refferal List

`Libs.ReferralLib.topList.clear()`

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

Libs.ReferralLib.currentUser.track({
   onAtractedByUser: onAttracted
});
```



**Q: how to give to referrer 5% of referral user deposit?**

**Answer:**

1. You need setup [track](https://help.bots.business/libs/refferallib#getting-started) in first
2. Seems you need use [ResourcesLib](https://help.bots.business/libs/resourceslib)
3. On user set balance:

```javascript
let res = Libs.ResourcesLib.userRes("money");
let referrer = Libs.ReferralLib.currentUser.AttractedByUser();

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

