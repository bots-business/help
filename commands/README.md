---
description: What it is "bot command"?
---

# Commands

Command - it is text from user. Bot can sent answer for command or do something. Usually command start with `"/"`, e.g. `/hello`. But it is not always required.

{% hint style="warning" %}
`/start` and `/START` - it is not same commands. Command is case sensitive
{% endhint %}

### How to execute command with any text from user? (Master command)

Just use `*` in command name.&#x20;

See [more](https://help.bots.business/scenarios-and-bjs/always-running-commands)



### Command's fields

Command can have:

| Field        | Description                                                                                                   | Example                                                                                                                    |
| ------------ | ------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `command`    | use for command call                                                                                          | <p><code>"/start"</code>, <code>"/run"</code>. For any text use <code>"*"</code></p><p><strong>case sensitive</strong></p> |
| `help`       | command's description                                                                                         | `"Welcome to RentBot. See /help or /order now"`                                                                            |
| `answer`     | text answer                                                                                                   | `"Hello. You need /register before continue"`                                                                              |
| `aliases`    | use for alternative command call                                                                              | `"/welcome, /hello, ?, help"`                                                                                              |
| `keyboard`   | send keyboard to user on command call                                                                         | `"order, about"`                                                                                                           |
| `scenarios`  | `BJS` code for execution                                                                                      | `2+2`                                                                                                                      |
| `group`      | command allowed only for this user's group                                                                    | `guests`, `clients`                                                                                                        |
| `need_reply` | command wait for answer from user. Can be true, false or blank. If `true` BJS code execute after users'answer | `true`, `false`                                                                                                            |
| `auto_retry` | command can be runs with interval in secs                                                                     | `600` - repeated once at 10 minutes                                                                                        |

### How to create and edit commands?

**You can edit command directly from application.**

![Screen from App for command creation](<../.gitbook/assets/image (97).png>)

### Commands importing

Make all commands with [Google Table. ](https://help.bots.business/create-bot-from-google-table)

Also you can copy Template table from [http://bit.ly/bb\_table\_template](http://bit.ly/bb\_table\_template) into your own table.&#x20;

This is an ideal option: everything is quite simple. Go to `Main menu > File > Make a copy`. You will need a separate sheet for the commands that will contain the commands. Then you can add commands in rows and do the CSV import from application.





###

