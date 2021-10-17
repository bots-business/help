---
description: With lib we can manage any resources in bot.
---

# ResourcesLib

## Resource can be

* balance (in USD, BTC or any other)
* any game resources: gold, woods, stone, etc
* etc, any float values

## User's resource

```javascript
let res = Libs.ResourcesLib.userRes("money");
Bot.sendMessage("Cur your money: " + res.value());
```

{% hint style="danger" %}
Res name is case sensitive. The resources “money”, “Money” and “MONEY” do not match. These are 3 separate resources.
{% endhint %}

{% hint style="info" %}
One user can have same chats with bot. 

**For example:** private and group chat. 

But anywhere he have **simular** resources
{% endhint %}

## Chat's resource

```javascript
let res = Libs.ResourcesLib.chatRes("money");
Bot.sendMessage("Cur your money: " + res.value());
```

{% hint style="info" %}
One user can have same chats with bot. 

**For example**: private and group chat.

But he have **diffent** resources for each chats.
{% endhint %}



## Methots for user's and chat resources

All methods can be for user's or chat's resources.

```javascript
// get res
let res = Libs.ResourcesLib.userRes("money");
```

`res.name` - current res name. For example: 

```javascript
Libs.ResourcesLib.chatRes("BTC").name // is "BTC"
```

## Basic functions

### Current res amount

`res.value()` 

### Set amount for this res 

`res.set(amount)` 

for example: `Libs.ResourcesLib.userRes("wood").set(10);`

### Add amount for this res

`res.add(amount)` 

### Res have such amount?

`res.have(amount)`- if res value equal amount or more return true

### Take away amount from resource

`res.remove(amount)` -  if have it res.removeAnyway(amount) - take away amount anyway.

## Access to another resources

### Access to another user's resources

```javascript
// telegramid - it is telegram id for another user
let res = Libs.ResourcesLib.anotherUserRes("money", telegramid);
Bot.sendMessage("Cur your money: " + res.value());
```

### Access to another chat's resources

```javascript
// another chat's resources
// chatid - it is telegram id for another chat
let res = Libs.ResourcesLib.anotherChatRes("money", chatid);
Bot.sendMessage("Cur your money: " + res.value());
```



## Resource transfering 

```javascript
let res = Libs.ResourcesLib.userRes("gold");
// telegramid - it is telegram id for another user
let anotherRes = Libs.ResourcesLib.anotherUserRes("gold", telegramid);
```

### If have resource...

```javascript
res.takeFromAnother(anotherRes, amount);
res.transferTo(anotherRes, amount)
```

### ...or anyway, even resource is not enough

```javascript
res.takeFromAnotherAnyway(anotherRes, amount)
res.transferToAnyway(anotherRes, amount)
```



### Can exchange different resources

For example "gold" for "wood":

`res.exchangeTo(anotherRes, { remove_amount: 10, add_amount:23 } )`

``

## Growth for resource.

Resource can have growth.

{% hint style="info" %}
For example simple growth:

**add 5 every 10 secs to res**
{% endhint %}

```javascript
let health = Libs.ResourcesLib.userRes("health");
health.set(1);
health.growth.add({value: 5, interval:10 });
```

Interval - it is value in seconds. Value is added every interval

### Add 5 every hour with max value 100.

```javascript
//Max value: 100
let secs_in_hour = 1 * 60 * 60;
health.growth.add({
  value: 5,
  interval: secs_in_hour,
  max: 100
});
```

### Value can be negative. Remove 5 every 30 hours. 

```javascript
//Min value: -20
let secs_in_30hours = 1 * 60 * 60 * 30;
health.growth.add({
  value: -5,  // just add negative value
  interval: secs_in_30hours,
  min: -20
});
```



### Can limit max iteration count

```javascript
health.growth.add(
   {value: 5,
   interval: secs_in_30hours,
   max_iterations_count: 3
});
```

### Can growh by percent. 

For example add 15% every month for 100 USD

```javascript
let usd = Libs.ResourcesLib.userRes("usd");
usd.set(100);
let secs_in_month = 60 * 60 * 24 * 31;
usd.growth.addPercent({
  value: 15,
  interval: secs_in_month
});
```



### Can grow by compound interest.

For example add 0.8% every day for 0.5 BTC with reinvest

```javascript
let btc = Libs.ResourcesLib.userRes("BTC");
btc.set(0.5);
let secs_in_day = 1 * 60 * 60 * 24;
usd.growth.addCompoundInterest({
  value: 0.8,
  interval: secs_in_day
});
```

{% hint style="info" %}
You can get initial res value by: `res.baseValue()`
{% endhint %}

### Other methods for res.growth: 

`res.growth.info()` - get info for current growth

`res.growth.title()` - get title. For example "add 5 once at 15 secs" 

`res.growth.isEnabled() `- return true if is enabled 

`res.growth.stop()` - stop growth 

`res.growth.progress()` - current progress for next iteration 

`res.growth.willCompletedAfter()` - will completed iteration after this time in seconds

###

### How to add growth to another resources?

For example we have:

* bank deposit 100$ with yearly growth 10%
* and simple wallet - 500$

Every year we add bank growth to wallet.

#### **Init: **on `/start` command (or any other command)

```javascript
let wallet = Libs.ResourcesLib.userRes("wallet");
wallet.set(500);

let bankDeposit = Libs.ResourcesLib.userRes("deposit");
bankDeposit.set(100);
let secs_in_year = 1 * 60 * 60 * 24 * 365;

bankDeposit.growth.addPercent({
  value: 10,
  interval: secs_in_year
});
```

#### **On** `/wallet` command or etc

{% hint style="info" %}
We can run this command every 1 year. It is possible for example, with [Auto Retry](https://help.bots.business/commands/auto-retry)

Or user can run it manually in anytime.
{% endhint %}

```javascript
let wallet = Libs.ResourcesLib.userRes("wallet");
let bankDeposit = Libs.ResourcesLib.userRes("deposit");

// it is initial res value
let baseValue = bankDeposit.baseValue();

// total income by percent
let delta = bankDeposit.value() - baseValue;

// add all income to wallet
wallet.add(delta);
// and remove it from bank deposit
bankDeposit.set(baseValue);
```





## How to

### **Q: How to give to referrer 5% of referral user deposit?**

Please see [https://help.bots.business/libs/refferallib#how-to](https://help.bots.business/libs/refferallib#how-to)



### **Q: How to give a bonus to all users every day?**

For example add 10 to user's balance every day

Command `/start`

```javascript
let balance = Libs.ResourcesLib.userRes("balance");
balance.set(0);

Bot.run( {
    command: "/addBonus",
    run_after: 1*60*60*24,  // add bonus after 1 day
} )
```

 Command `/addBonus`

```javascript
if(request){
  // user can not run this command manually
  Bot.sendMessage("Restricted!")
  return
}

let balance = Libs.ResourcesLib.userRes("balance");
balance.add(10);

// and repeat this command again after 1 day 
Bot.run( {
    command: "/addBonus",
    run_after: 1*60*60*24,  // after one day
} )

Bot.sendMessage("Bonus for you: 10")
```

{% hint style="warning" %}
Command /addBonus will be executed for each user. It spend 1 iteration every day for each user.

For example, for 100 user - it will be 100 iterations per day.
{% endhint %}

