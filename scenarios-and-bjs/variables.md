# المتغيرات

| متغير | الوصف |
| : --- | : --- |
| طلب | إنها مجموعة JSON مع الكثير من البيانات. يمكنك أن ترى ذلك من قبل
`Bot.sendMessage (فحص (طلب))` |
| رسالة | الرسالة الحالية من سلسلة المستخدم |
| المحتوى | محتوى تم تنزيله \ (إذا كان هناك عنوان URL للتحميل في السيناريو \) - النص |
| مستخدم | المستخدم الذي حد ذاته
Fields: `id`, `first_name`, `last_name`, `username`, `telegramid`, `created_at`, `updated_at` |
| الدردشة | البيانات الحالية chat. Fields: `id`, `chatid`, `title`, `chat_type` \(يمكن ان يكون: "private", "group `botan_token`, `last_run_at`, `store_bot_id`, `status` |
| params | معلمات الأوامر - النص |
| statistics | إحصائيات بوت. Fields: `total`, `group_chats_count`, `user_chats_count`, `super_group_chats_count`, `active_during_last_day`, `active_during_last_week` |
| completed\_commands\_count | عدد الأوامر المكتملة مسبقًا في `Bot.runCommand` calls.
قابل للاستخدام ل [security](https://help.bots.business/scenarios-and-bjs/bjs-security#use-completed_commands_count-variable) |

### يمكنك فحص أي متغير للتصحيح

```text
Bot.sendMessage( inspect(user) )
Bot.sendMessage( inspect(chat) )
```

