---
description: Преобразование так же просто, как несколько строк кода!
---

# Конвертер валют

Значения валют обновляются каждые 60 минут.

## БЕСПЛТАНО

* Максимум 100 запросов/час
* Поделиться со всеми пользователями, которые получают доступ к нему бесплатно.
* Время застоя, когда необходимо перезапустить сервер для исправления ошибок и улучшения

## Пример бота @DemoCurrencyConverterBot

## Пример кода

В любой команде:

```javascript
let amount = 1;
let onSucces = '/onconvert';
let conversation = 'USD_EUR' // другие: USD_BTC, BTC_USD, CNY_BTC и т.д....
Libs.CurrencyConverter.convert(conversation, amount, onSucces);
```

### В команде '/onconvert':

```javascript
// результат записывается в параметрах
Bot.sendMessage(params);
```



