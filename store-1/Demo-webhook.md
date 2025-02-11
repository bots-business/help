
# BBWebhookBot  

This bot demonstrates how the **Webhook Library** works, its purpose, and how to implement it in your projects.  

## What is a Webhook?  

A **webhook URL** is **unique for every user** and is completely **secure**. You can generate a webhook URL for a specific user and for a **specific command**.  

When a **POST or GET request** is received, the command associated with the webhook URL will run **automatically**, and all the received data will be stored in the `"content"` predefined variable.  

### Generate a Webhook URL  
```js
// user's webhook
let webhookUrl = Libs.Webhooks.getUrlFor({
  // this command will be runned on webhook
  command: "/onWebhook",
  // this text will be passed to command
  content: "Did you see the cat?",
  // execute for this (current) user
  user_id: user.id,
  // redirect to page with cat after calling webhook
  // you need remove this for external service
  redirect_to: "https://cataas.com/cat"
})

Bot.inspect(webhookUrl);
```

📖 **More about the library: [Visit this page](libs/webhooks-lib.md)**  

---

## How This Bot Works  

This bot generates a **webhook URL** with two key parameters:  
- **`content`** (contains the received data)  
- **`redirect_to`** (a URL where users will be redirected)  

**Example:**  
![Image here](/.gitbook/assets/webhookBot.png)

When you open the webhook URL, it will **redirect you** to the **`redirect_to`** URL while simultaneously sending you the text stored in the `"content"` variable.  

---

## Managing Webhook Content  

The content received via the webhook is **processed by the command** used to generate it.  

- The **command runs automatically** whenever data is received.  
- The bot then **sends you the received content** for further processing.  

---

## Real-World Use Cases  

### Example 1: **Web Page Integration**  
- Visit the demo page to **get your webhook URL**.  
- Send data using a **POST or GET request**.  
- The bot will receive the data in the `"content"` variable, allowing you to process it as needed.  

### Example 2: **Telegram Alerts for Website Events**  
If you want to receive **real-time alerts on Telegram** when a new user registers on your website:  
- Call the **webhook URL** from your website.  
- Include the user's details in the request body.  
- The bot will **receive the data in the `"content"` variable** and send an alert to you.  

### Example 3: **WebApp Integration**  
You can integrate **web apps** with the bot using webhook URLs.  

For example, in a **game WebApp**:  
- When a user finishes a game, send their **score** to the webhook URL.  
- The bot receives the score in the `"content"` variable.  
- The bot saves the score under the **user's profile** for tracking.  

---

## Working Example: **@BBPointBot**  
The **@BBPointBot** uses the webhook system as an API to handle:  
- **BB Point transfers**  
- **Data processing**  
- **Automated requests**  

---

With **BBWebhookBot**, you can create **powerful automations** and integrate your bot with **external applications, websites, and web apps** seamlessly!
