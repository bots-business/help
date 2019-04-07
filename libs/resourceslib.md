---
description: С помощью библиотеки мы можем управлять любыми ресурсами в боте.
---

# ResourcesLib

## Ресурсы могут быть

* в виде баланса \(в USD, BTC или других\)
* любые игровые ресурсы: золото, древесина, камень, и т.д.
* или, любые цифровые значения

## Ресурс пользователя

```javascript
let res = Libs.ResourcesLib.userRes("money");
Bot.sendMessage("Недавний ваш счет: " + res.value());
```

{% hint style="info" %}
Пользователь может иметь такие же чаты с ботом. 

**Например:** личный и групповой чат. 

Но везде он имеет **одинаковые** ресурсы
{% endhint %}

## Ресурсы чатов

```javascript
let res = Libs.ResourcesLib.chatRes("money");
Bot.sendMessage("Cur your money: " + res.value());
```

{% hint style="info" %}
Пользователь может иметь такие же чаты с ботом. 

**Например:** личный и групповой чат. 

Но, он имеет **одинаковые** ресурсы для каждого чата.
{% endhint %}



## Методы для ресурсов пользователя и чата

Все методы могут быть для пользователя или чата.

```javascript
// get res
let res = Libs.ResourcesLib.userRes("money");
```

`res.name` - недавнее название ресурса. Например: 

```javascript
Libs.ResourcesLib.chatRes("BTC").name // is "BTC"
```

## Основные функции

### Недавнее количество ресурсов

`res.value()` 

### Установить количество для этого ресурса 

`res.set(amount)` 

например: `Libs.ResourcesLib.userRes("wood").set(10);`

### Добавить количество для этого ресурса

`res.add(amount)` 

### Ресурс имеет такую сумму?

`res.have(amount)`- если значение ресурса равно или больше, возвращается true

### Забрать сумму с ресурса

`res.remove(amount)` -  если имеет это res.removeAnyway\(количество\) - забирает сумму в любом случае.

## Доступ к другому ресурсу

### Доступ к ресурсам другого пользователя

```javascript
// telegramid - это идентификатор телеграма для другого пользователя
let res = Libs.ResourcesLib.anotherUserRes("money", telegramid);
Bot.sendMessage("Недавний ваш счет: " + res.value());
```

### Доступ к ресурсам другого чата

```javascript
// ресурсы другого чата
// chatid - это телеграм идентификатор для другого чата
let res = Libs.ResourcesLib.anotherChatRes("money", chatid);
Bot.sendMessage("Недавний ваш счет: " + res.value());
```



##  Передача ресурсов

```javascript
let res = Libs.ResourcesLib.userRes("gold");
// telegramid - это телеграм идентификатор другого пользователя
let anotherRes = Libs.ResourcesLib.anotherUserRes("gold", telegramid);
```

### Если есть ресурс...

```javascript
res.takeFromAnother(anotherRes, amount);
res.transferTo(anotherRes, amount)
```

### ...или в любом случае, даже ресурса недостаточно

```javascript
res.takeFromAnotherAnyway(anotherRes, amount)
res.transferToAnyway(anotherRes, amount)
```



### Можно обменять разные ресурсы

Например "золото" на "древесину":

`res.exchangeTo(anotherRes, { remove_amount: 10, add_amount:23 } )`

\`\`

## Рост ресурса.

Ресурс может иметь рост.

{% hint style="info" %}
Например простой рост:

**добавить 5 к ресурсу каждые 10 секунд**
{% endhint %}

```javascript
let health = Libs.ResourcesLib.userRes("health");
health.set(1);
health.growth.add({value: 5, interval:10 });
```

Interval(интервал) - это значение в секундах. Значение добавляется каждый определенный интервал

### добавить 5 к ресурсу каждый час с максимальным значение 100.

```javascript
//максимальное значение: 100
let secs_in_hour = 1 * 60 * 60;
health.growth.add({
  value: 5,
  interval: secs_in_hour,
  max: 100
});
```

### Значение может быть отрицательным. Отнимать 5 каждые 30 часов. 

```javascript
//мин. значение: -20
let secs_in_30hours = 1 * 60 * 60 * 30;
health.growth.add({
  value: 5,
  interval: secs_in_30hours,
  min: -20
});
```



### Можно ограничивать максимальное количество итераций

```javascript
health.growth.add(
   {value: 5,
   interval: secs_in_30hours,
   max_iterations_count: 3
});
```

### Возможно увеличение в процентах. 

Например добавить 15% каждый месяц на 100 USD

```javascript
let usd = Libs.ResourcesLib.userRes("usd");
usd.set(100);
let secs_in_month = 60 * 60 * 24 * 31;
usd.growth.addPercent({
  value: 15,
  interval: secs_in_month
});
```



### Может расти на сложный процент.

Например добавить 0.8% каждый день на 0.5 BTC с реинвестицией

```javascript
let btc = Libs.ResourcesLib.userRes("BTC");
btc.set(0.5);
let secs_in_day = 1 * 60 * 60 * 24;
usd.growth.addCompoundInterest({
  value: 0.8,
  interval: secs_in_day
});
```

{% hint style="info" %}
Вы можете получить начальное значение res с помощью: 
`res.baseValue ()`
{% endhint %}

### Другие методы для res.growth: 

`res.growth.info()` - получить инфо. за недавний рост
`res.growth.title()` - получить оглавление. Например "добавить 5 раз в 15 секунд" 

`res.growth.isEnabled()` - возвращает true если включен 

`res.growth.stop()` - остановить авторост 

`res.growth.progress()` - недавний прогресс для следующей итерации

`res.growth.willCompletedAfter()` - будет завершена итерация после этого времени в секундах

### 

### Как добавить рост к другим ресурсам?

Например, у нас есть:

* банковский депозит 100 $ с годовым ростом 10%
* и простой кошелек - 500 $

Каждый год мы добавляем баковский рост в кошелек.

#### **Начало:** при команде `/start` \(или любая другая команда\)

```javascript
let wallet = Libs.ResourcesLib.userRes("wallet");
wallet.set(500);

let bankDeposit = Libs.ResourcesLib.userRes("deposit");
bankDeposit.set(100);
let secs_in_year = 1 * 60 * 60 * 24 * 365;

bankDeposit.growth.addPercent({
  value: 10,
  interval: secs_in_year
});
```

#### **При** команде `/wallet` или др.

{% hint style="info" %}
Мы можем запустить эту команду каждый год. Это возможно например, с помощью [Автовоспроизведения](https://help.bots.business/commands/auto-retry)

Или другой пользователь может запустить это вручную в любое время.
{% endhint %}

```javascript
let wallet = Libs.ResourcesLib.userRes("wallet");
let bankDeposit = Libs.ResourcesLib.userRes("deposit");

// это начальное значение ресурса
let baseValue = bankDeposit.baseValue();

// общий доход по процентам
let delta = bankDeposit.value() - baseValue;

// перевести весь доход в кошелек
wallet.add(delta);
// и убрать это с банковского депозита
bankDeposit.set(baseValue);
```





## Как...

**Q: Как дать пригласителю 5% от депозита реферального пользователя?**

Пожалуйста см. [https://help.bots.business/libs/refferallib\#how-to](https://help.bots.business/libs/refferallib#how-to)

