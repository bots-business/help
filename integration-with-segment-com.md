# Интеграция с Segment.com

## Интеграция с Segment.com

### 1. Создайте аккаунт в Segment.com и авторизуйтесь: [https://app.segment.com/login](https://app.segment.com/login)

### 2. Откройте рабочее место

### 3. Нажмите на "Sources"

[![](https://camo.githubusercontent.com/3edc620a7b362530a6724b30cc94de002177dc1d/68747470733a2f2f692e696d6775722e636f6d2f6e574c657a74642e706e67)](https://camo.githubusercontent.com/3edc620a7b362530a6724b30cc94de002177dc1d/68747470733a2f2f692e696d6775722e636f6d2f6e574c657a74642e706e67)

### 4. Нажмите на "Add source"

[![](https://camo.githubusercontent.com/d1da8718cbbebc401d008f658605937b18a48d48/68747470733a2f2f692e696d6775722e636f6d2f595243414146452e706e67)](https://camo.githubusercontent.com/d1da8718cbbebc401d008f658605937b18a48d48/68747470733a2f2f692e696d6775722e636f6d2f595243414146452e706e67)

### 5. Наберите "Ruby" в поле для поиска, затем выберите "Ruby"

[![](https://camo.githubusercontent.com/fe5f49dfb73264335235952b1eddc57e2a2a1bdb/68747470733a2f2f692e696d6775722e636f6d2f554d757766636a2e706e67)](https://camo.githubusercontent.com/fe5f49dfb73264335235952b1eddc57e2a2a1bdb/68747470733a2f2f692e696d6775722e636f6d2f554d757766636a2e706e67)

### 6. Нажмите на кнопку "Connect"

[![](https://camo.githubusercontent.com/ed6f1023450c3571d0c4bb5f7c9f666dc6759e86/68747470733a2f2f692e696d6775722e636f6d2f583478674875572e706e67)](https://camo.githubusercontent.com/ed6f1023450c3571d0c4bb5f7c9f666dc6759e86/68747470733a2f2f692e696d6775722e636f6d2f583478674875572e706e67)

### 7. Наберите любое название и нажмите на "Add Source"

[![](https://camo.githubusercontent.com/76cfcaeeb00e94989ae37694088469fe229fb74a/68747470733a2f2f692e696d6775722e636f6d2f5468796d33504f2e706e67)](https://camo.githubusercontent.com/76cfcaeeb00e94989ae37694088469fe229fb74a/68747470733a2f2f692e696d6775722e636f6d2f5468796d33504f2e706e67)

### 8. Скопируйте "Write Key"

[![](https://camo.githubusercontent.com/8f09fbf314da0b6106a61e597b0719c95c1f119f/68747470733a2f2f692e696d6775722e636f6d2f6466476e6c46662e706e67)](https://camo.githubusercontent.com/8f09fbf314da0b6106a61e597b0719c95c1f119f/68747470733a2f2f692e696d6775722e636f6d2f6466476e6c46662e706e67)

### 9. Откройте приложение Bots.Business. Выберите бота -&gt; Изменить. Вставьте "Write Key"(ключ из segment.com)

[![](https://camo.githubusercontent.com/dffbae5ced5459082dd294c5cc5a2f2fb8b82bb6/68747470733a2f2f692e696d6775722e636f6d2f47307742366c532e706e67)](https://camo.githubusercontent.com/dffbae5ced5459082dd294c5cc5a2f2fb8b82bb6/68747470733a2f2f692e696d6775722e636f6d2f47307742366c532e706e67)

### 10. Перезапустите бот

### 11. Отслеживание

Автоматическое отслеживание: новых пользователей, последнее присутствие пользователя

BJS пример: `Segment.trackEvent({ event: 'Ordered', properties: {title: 'mega product', price: 10 } })`

