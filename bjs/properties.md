# Properties

## Introducing

It is possible to save data in bot. Data can be numeric, text, datetime and etc.

For example:

* Bot can have DailyBonus saved in bot prop (it is global for all bot)
* User can have Balance saved in user prop (it is personall)

{% hint style="info" %}
Properties can be belong for user or for bot.
{% endhint %}

## Types

Properties can be:

* integer, e.g.: `150`
* float, e.g.: `5.5`&#x20;
* boolean: `true, false`
* string, e.g.: `"Hello world"`
* text, e.g.: `"it is very big text ..... 1000 symbols here or more"`
* json, e.g.: `{ any: "data", here: "can", be: {used: true}, with: ["array", "too"] }`
* datetime, e.g: `new Date()`



## Set property

```javascript
// set global prop
Bot.setProperty({ name: 'myProp', value: 15 });
 
// set JSON prop for user
User.setProperty({
 name: 'BIO',
 value: { email: "test@example.com", age: 10 }
});
```

{% hint style="success" %}
* so for global prop use `Bot.xxx` method
* for user's prop use `User.xxx` method
{% endhint %}

also you can use old style:

```javascript
 // set global prop
Bot.setProperty({ name: 'myProp', value: 15, type: "float" });
```

### Set prop for other user

```javascript
 // set global prop
Bot.setProperty({
  name: 'otherUserProp',
  value: "test Prop",
  user_id: other_user.id // you can use user.id for saving user prop for cur user
});
```

### You can save prop in the List

Please read this [article](lists/)



