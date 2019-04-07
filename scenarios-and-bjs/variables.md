# Переменные

| Переменная | Описание |
| :--- | :--- |
| request(запрос) | это сборник JSON со множеством данных . Вы можете увидеть их с помощью `Bot.sendMessage( inspect(request) )` |
| message(сообщение) | текущее сообщение от пользователя - строка |
| content | скачанный контент \(если в сценарии был URL для загрузки\) - текст |
| user(пользователь) | пользователь который отправил команду или текст. Поля: `id`, `first_name`, `last_name`, `username`, `telegramid`, `created_at`, `updated_at` |
| chat(чат) | данный для текущего чата. Поля: `id`, `chatid`, `title`, `chat_type` \(могут быть: "private", "group", "supergroup"\), user\_id, created\_at, updated\_at, bot\_id |
| bot(бот) | данные бота. Поля: `id`, `name`, `token`, `created_at`, `updated_at`, `csv_url`, `botan_token`, `last_run_at`, `store_bot_id`, `status` |
| params(параметры) | параметры команд - text |
| statistics(статистика) | статистика бота. поля: `total`, `group_chats_count`, `user_chats_count`, `super_group_chats_count`, `active_during_last_day`, `active_during_last_week` |
| completed\_commands\_count(количество завершенных команд) | количество, до данного времени, запущенных команд при помощи вызова `Bot.runCommand` |

### Вы можете проверить любую переменную для отладки

```text
Bot.sendMessage( inspect(user) )
Bot.sendMessage( inspect(chat) )
```

