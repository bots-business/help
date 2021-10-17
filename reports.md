# Reports

You can see a usage report and a commands report for your bot. Just click to links.

![](<.gitbook/assets/image (83).png>)

## Usage report

shows the count of new users, chats, received and sent messages, blocked chats, etc. 

![](<.gitbook/assets/image (85).png>)

## Commands report

with this report you can see:

* slowest commands (Average run time option)
* the most used commands (Total calls option)

Command can execute with API, BJS-runtime and in background

API - this is an external API call. For example Telegram API

BJS-runtime - this is the execution of your JavaScript code

Background - this is the execution of Bot.run with run_after options and another scheduled tasks.

![](<.gitbook/assets/image (86).png>)

{% hint style="warning" %}
Average run time for API must be above 0.1 - 0.5 sec. If your command spends more time you need to optimize it.
{% endhint %}
