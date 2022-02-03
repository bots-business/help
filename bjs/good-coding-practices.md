# Good coding practices

## Use Libs

BB have good [libs](broken-reference). Just install needed library to your bot.

{% hint style="warning" %}
Don't install unnecessary libraries. Your bot might be slow after that.
{% endhint %}

## Use folders

You can organize your commands in folders.&#x20;

![](<../.gitbook/assets/image (90) (1) (1).png>)

You can use folder in BJS too. For example in [before All](always-running-commands.md#beforeall-and-afterall-commands) command:

```javascript
// Before all command - @

// set your ADMIN_ID here
// you can get it via Bot.sendMessage(user.id)
var isAdmin = ( user && (user.id == ADMIN_ID) )

if(!command){
   return
}

if((command.folder=="Admin Panel")&&(isAdmin){
  // only admin can run command from Admin Panel's folder
  // any common bjs here for admin
  Bot.sendMessage("Hello, admin!")
}else{
  Bot.sendMessage("Access denied");
  return // exit from command now
}

// other example
if(command.folder=="Under development"){
  Bot.sendMessage("Sorry this command on development")
}
```

## Use good names

Do you have children.

Two boys and one girl. You do not call them b1, b2 ang g1, are you not ?

So why do you call your variables like that?

**Bad examples:**

* x1
* y2
* d3

**Good:**

* currentLimit
* maxCount
* userName

## Use simular names!

![](<../.gitbook/assets/image (58).png>)

## Use JSON property

**Bad code:**

![](<../.gitbook/assets/image (60).png>)

****

**Good code:**

![](<../.gitbook/assets/image (61).png>)

## Do not use many methods of getProperty or setProperty

**Bad code:**

![](<../.gitbook/assets/image (65).png>)

{% hint style="warning" %}
Each getProperty method spent above 10-100 ms for execution. So if you have 10 getProperty methods bot can spent above 1 sec for execution! It is slowly
{% endhint %}

**Use JSON type:**

![](<../.gitbook/assets/image (66).png>)

## Use functions!

Do not repeat youself!

Bad code:![](https://telegra.ph/file/31bc228cf1f6f793ff034.png)

![](<../.gitbook/assets/image (64).png>)



Good code:![](https://telegra.ph/file/aa3021f92fbd73e5c9ede.png)

![](<../.gitbook/assets/image (63).png>)
