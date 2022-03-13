# Protected bot

Protected bot - it is a bot without access to commands editing and copying.

{% hint style="success" %}
Protected bots - good thing for selling to customers.

Make protected bots with [Admin Panels](https://help.bots.business/scenarios-and-bjs/admin-panel) and sell them
{% endhint %}



**Forbidden for this bot:**

* add new command
* edit and remove commands
* view commands
* change BJS
* make bot copy (via app and BJS)
* git exporting (via app and BJS). Git importing is allowed

**Allowed:**

* Bot token changes
* Bot name changes
* Admin panel: view + changing
* Properties: view + changing
* Errors
* Chats, users

{% hint style="danger" %}
**Security**

Try to remove all secure data in such bot like:

* BB Api key
* Coinpayment api keys
* etc



Strictly **do NOT** use such safe data in `/setup`

It will be avaible in Props Tab!



Also, your secure data can be available in Error Tab. So try don't use safe data in protected bot.
{% endhint %}



## How to create Protected bot?

If you want to sell your bot - just start develop it.

You can [install protected bot](https://help.bots.business/scenarios-and-bjs/bb-admin-functions#bbadmin-installbot) for another users via BJS

Contact the administrator to start selling this bot.
