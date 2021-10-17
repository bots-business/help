# Variables

In BJS we have useful global variables.

| **Variable**             | **Description**                                                                                                                                                                                   |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| request                  | it is JSON collection with a lot of data. You can see it by `Bot.sendMessage( inspect(request) )`                                                                                                 |
| message                  | current message from user - string                                                                                                                                                                |
| user                     | user who sent a command or text. Fields: `id`, `first_name`, `last_name`, `username`, `telegramid`, `created_at`, `updated_at`                                                                    |
| chat                     | data for current chat. Fields: `id`, `chatid`, `title`, `chat_type` (can be: "private", "group", "supergroup"), user_id, created_at, updated_at, bot_id                                           |
| bot                      | data for bot. Fields: `id`, `name`, `token`, `created_at`, `updated_at`, `csv_url`, `botan_token`, `last_run_at`, `store_bot_id`, `status`                                                        |
| command                  | data for command. Fields: `id`, `name`, `folder`, `need_reply`, `auto_retry_time`, `last_auto_retry_at`, `created_via_csv_import`, `last_csv_import_at`, `created_at`, `updated_at`               |
| params                   | command parameters - text                                                                                                                                                                         |
| owner                    | information about bot owner: email, id and etc                                                                                                                                                    |
| completed_commands_count | the count of previously completed commands on `Bot.runCommand` calls. Can be used for [security](https://help.bots.business/scenarios-and-bjs/bjs-security#use-completed_commands_count-variable) |
| iteration_quota          | current quota information: limit, progress and etc. You can see it by `Bot.sendMessage( inspect(iteration_quota) )`                                                                               |
| payment_plan             | current bot owner payment plan information                                                                                                                                                        |
| BB_API_URL               | api url: api.bots.business. Each Cloud have own Api Url                                                                                                                                           |

###

### You can inspect any variable for debug

```
Bot.sendMessage( inspect(user) )
Bot.sendMessage( inspect(chat) )
```
