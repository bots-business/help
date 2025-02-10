# Demo Resources Bot  

This bot demonstrates how to work with the **Resources Library (resLib)**—a simple library for managing resources in Telegram bots. The library is **exclusive to Bot.Business bot builder**.  

## What is the Resources Library?  

resLib allows you to **store, modify (add, remove, set), and manage any resources** in your bot. You can use **custom resource names** like `"crystal"`, `"usdt"`, `"balance"`, etc.  

With resLib, you can:  
- Modify resources for any user using their **Telegram ID**.  
- Set **growth** for resources, meaning the resource amount **automatically increases or decreases** over time.  

> Growth Example: You can define how often a resource increases, its maximum amount, and time intervals. This is useful for **investment or loan bots** where resources change periodically.  

📖 **Read more about resLib [here](https://github.com/nasirul786/help/blob/master/libs/resourceslib.md).**  

---

## How This Bot Works  

This bot **demonstrates how to use resLib effectively**. It includes **five different resources**, but you can add **any number of resources** in your own bot.  

- Some resources are used for **counting or managing settings** (e.g., bank deposits).  
- Others act as **currencies**, like `"crystal"` and `"usd"`.  

---

## Using resLib in Your Bot  

### Add a Specific Amount to a User's Resources  
```js
//define resLib.
var balance = Libs.ResourcesLib.userRes("balance");
balance.add(100)
/* We can use -100 for removing 100 from balance resources, or we can use like this:
balance.remove(100); too *\
```

### Set a Fixed Amount for a Resource  
> **Note:** This will override the user's current balance, setting it to the new amount.  
```js
//define resLib.
var balance = Libs.ResourcesLib.userRes("balance");
balance.set(100)
// This method will reset the balance to ybe provided amount
```

### Add Growth to a Resource  
(code here)  

The **growth feature** is essential for **investment or loan bots**, where resources **increase or decrease** at scheduled intervals.  

For example:  
- If you set the growth interval to **5 minutes**, the resource will **increase or decrease** every 5 minutes.  
- You can set a **maximum limit** to prevent excessive growth.  

📌 **More Info:** Visit the **Resources Library page** for detailed documentation.  

---

## Get the Demo Resources Bot  

🔗 **See the bot on the Bot Store** or check out the **GitHub repo** to view the full source code.  
- You can **copy, modify, and implement** the code for your own projects!
