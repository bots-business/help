# BlockIO

Эта библиотека делает интеграцию с [block.io](blockio.md) удобной.

![](../.gitbook/assets/image%20%2829%29.png)

## С помощью Block.io вы можете:

* создать новый кошелек: Bitcoin, Dogecoin, Litecoin
* посмотреть транзакции
* получить доступ к оплате
* сделать вывод
* и т.д.

## Демо бот

Смотреть [DemoBlockIOBot](https://telegram.me/DemoBlockIOBot). It also available in the Store.

Смотреть [документы](https://help.bots.business/store/blockiobot) about this bot.



## Начальная настройка

Необходимо установить Api ключ и секретный шрифт, для начала. Вы можете создать команду `/init` с помощью этого BJS:

```javascript
// установка секретного шрифта(PIN)
Libs.BlockIO.setSecretPin("ВАШ ПИН-КОД");

Libs.BlockIO.Bitcoin.setApiKey("ВАШ_API_КЛЮЧ ДЛЯ Bitcoin");
Libs.BlockIO.Dogecoin.setApiKey("ВАШ_API_КЛЮЧ для Dogecoin");
Libs.BlockIO.Litecoin.setApiKey("ВАШ_API_КЛЮЧ для Dogecoin");

// Testnet
Libs.BlockIO.testNet.Bitcoin.setApiKey("YOURS_API_KEY for Bitcoin");
Libs.BlockIO.testNet.Dogecoin.setApiKey("YOURS_API_KEY for Dogecoin");
Libs.BlockIO.testNet.Litecoin.setApiKey("YOURS_API_KEY for Dogecoin");
```

Просто запустите бота и выведите эту команду `/init`. Затем вы можете убрать этo.

{% hint style="warning" %}
Вы получите секретный ПИН-код с Block.io через электронную почту
{% endhint %}

{% hint style="warning" %}
Вы получите API ключи в Block.io [доске](https://block.io/dashboard) в ссылке "Show API Keys"
{% endhint %}

{% hint style="success" %}
Вы также можете предоставить ПИН-код и api ключ в опциях команд
{% endhint %}

## Методы

Все методы из [https://block.io/api](https://block.io/api/simple/) доступны в библиотеке.

Вы можете запустить их через команду :

```javascript
Libs.BlockIO.XXXcoin.methodYYY(
    { onSuccess: "/onNewAddress",
      onError: "/onerror",
      // ....
      // ДРУГИЕ ВОЗМОЖНЫЕ ПАРАМЕТРЫ - смотрите позже
} );
```

{% hint style="info" %}
XXXCoin - это Bitcoin, Litecoin или Dogecoin
{% endhint %}

{% hint style="success" %}
Вы также можете использовать Testnet: Libs.BlockIO.testNet.XXXcoin
{% endhint %}

### Все методы

<table>
  <thead>
    <tr>
      <th style="text-align:left">Название метода
        <br />Libs.BlockIO.XXXcoin</th>
      <th style="text-align:left">Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">getNewAddress</td>
      <td style="text-align:left">Возвращает ново-сгенерированный адрес, и он уникален(!) метка сгенерированная
        Block.io. По желанию, Вы можете указать пользовательский ярлык.</td>
    </tr>
    <tr>
      <td style="text-align:left">getBalance</td>
      <td style="text-align:left">Возвращает баланс всего вашего Bitcoin, Litecoin, или Dogecoin аккаунта
        (например, сумму счетов всех адресов/пользователей  the sum of balances of all addresses/users внутри) в виде цифр
        до 8 десятичных знаков, как строка.</td>
    </tr>
    <tr>
      <td style="text-align:left">getAddressBalance</td>
      <td style="text-align:left">
        <p>Возвращает баланс определеных адресов или ярлыков. До 2500 адресов/ярлыков
          могут быть определеные за каждый запрос.
          <br />
        </p>
        <p>Может использоваться для запроса баланса для внешних (не связанных с аккаунтом) адресов. Если
          внешний адрес' баланс возвращается, его поля <em>user_id</em> и <em>ярлык</em>
          будут <em>без значений</em>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">getMyAddresses</td>
      <td style="text-align:left">Возвращает (неархивированные) адреса, их метки, идентификаторы пользователей и балансы.
        на вашем счету. До 2500 адресов на странице. Параметр страницы не является обязательным.</td>
    </tr>
    <tr>
      <td style="text-align:left">getAddressByLabel</td>
      <td style="text-align:left">Возвращает адрес, указанный ярлыком.</td>
    </tr>
    <tr>
      <td style="text-align:left">isValidAddress</td>
      <td style="text-align:left">Возвращает, является ли единственный указанный адрес действительным для сети или
        нет.</td>
    </tr>
    <tr>
      <td style="text-align:left">archiveAddresses</td>
      <td style="text-align:left">Архивирует до 100 адресов за один вызов API. Адреса могут быть указаны
        по их ярлыкам.</td>
    </tr>
    <tr>
      <td style="text-align:left">unarchiveAddresses</td>
      <td style="text-align:left">Разархивирует до 100 адресов за один вызов API. Адреса могут быть указаны
        по их ярлыкам.</td>
    </tr>
    <tr>
      <td style="text-align:left">getMyArchivedAddresses</td>
      <td style="text-align:left">Возвращает все <em> архивированные </ em> адреса, их метки и идентификаторы пользователей.
        на вашем счету</td>
    </tr>
    <tr>
      <td style="text-align:left">getTransactions</td>
      <td style="text-align:left">
        <p>Возвращает различные данные за последние 25 проведенных или полученных транзакций. Вы
          при желании можете указать параметр <em> before_tx </ em> для получения более ранних транзакций.</p>
        <p></p>
        <p>Вы можете использовать этот метод для запроса адресов, которых нет в вашей учетной записи.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">getRawTransaction</td>
      <td style="text-align:left">Возвращает необработанные данные, включая шестнадцатеричную транзакцию, ID для данной
        транзакции.</td>
    </tr>
    <tr>
      <td style="text-align:left">getNetworkFeeEstimate</td>
      <td style="text-align:left">Оценивает сетевую плату, которую вам нужно будет заплатить за запрос вывода
        средств. Сетевой сбор требуется биткойнами / Dogecoin / и т.д. сети,
        не Block.io.</td>
    </tr>
    <tr>
      <td style="text-align:left">isGreenTransaction</td>
      <td style="text-align:left">Возвращает массив транзакций, которые были отправлены зелеными адресами Block.io.
        Средства, отправленные с Зеленых адресов, гарантированы Block.io и могут быть
        использованы сразу при получении с нулевыми подтверждениеми сети.</td>
    </tr>
    <tr>
      <td style="text-align:left">getCurrentPrice</td>
      <td style="text-align:left">Возвращает цены из крупнейших бирж для биткойнов, дожекойнов или
        Litecoin, указанный в API Key. Указывать базовую валюту необязательно.</td>
    </tr>
    <tr>
      <td style="text-align:left">withdraw</td>
      <td style="text-align:left">Снимает количество монет с любых адресов в вашем аккаунте до
        2500 адресов.</td>
    </tr>
    <tr>
      <td style="text-align:left">withdrawFromAddresses</td>
      <td style="text-align:left">Снимает КОЛИЧЕСТВО монет с 2500 адресов одновременно и вносит депозиты
        до 2500 адресов.</td>
    </tr>
    <tr>
      <td style="text-align:left">withdrawFromLabels</td>
      <td style="text-align:left">Извлекает КОЛИЧЕСТВО монет с 2500 ярлыков одновременно и вносит их
        до 2500 адресов или меток.</td>
    </tr>
  </tbody>
</table> Полный документ методов [здесь](https://block.io/api/)

Так что если вам нужен, например, метод getTransactions для Litecoin, измените этот код:

```javascript
Libs.BlockIO.XXXcoin.methodYYY(
    { onSuccess: "/onNewAddress",
      onError: "/onerror",
      // ....
      // ДРУГИЕ ВОЗМОЖНЫЕ ПАРАМЕТРЫ
} );
```

to:

```javascript
Libs.BlockIO.Litecoin.getTransactions(
    { onSuccess: "/onGetTransaction",
      onError: "/onerror",
      // ....
      // ДРУГИЕ ВОЗМОЖНЫЕ ПАРАМЕТРЫ
} );
```

### Что это такое - "ДРУГИЕ ВОЗМОЖНЫЕ ПАРАМЕТРЫ" ?

Это могут быть:

* api\_key и пин-код. Если вы не установили [здесь](https://help.bots.business/libs/blockio#initial-setup)  \(для всех методов\)
* метка, адрес \(для методов getNewAddress и др.\)
* метки, адреса
* to\_addresses, from\_addresses, from\_labels \(для вывода \)
* страница
* transaction\_ids \(для метода isGreenTransaction \)
* type \(для метода getTransactions\)

{% hint style="info" %}
Смотрите возможные параметры на странице [https://block.io/api](https://block.io/api/)
{% endhint %}

###

### Например для getNewAddress

Например: "Действия для обработки адресов"

Помощь [block.io](https://block.io/api/):

![из справки Block.io - https://block.io/api](../.gitbook/assets/image%20%2825%29.png)

Итак, у нас есть метод API «Get New Address(Получить новый адрес)»:

```javascript
Libs.BlockIO.Bitcoin.getNewAddress(
    { onSuccess: "/onNewAddress",
      onError: "/onerror"
} );
```

Эта функция getNewAddress имеет 2 обратных вызова: `onSuccess` и `onError`.

вызов `onSuccess` запускает команду `/onNewAddress`:

```javascript
let wallet = options;  // у нас есть ответ Block.io в настройках

// раскомментируйте эту строку для проверки всех вариантов
// Bot.sendMessage(inspect(options));

Bot.sendMessage(
    "*Новый кошелек* был создан. \n  #вѓЈКошелет: `" +
    wallet.address + "`\n  рџ·Метка: `" +
    wallet.label + "`"
);
```



### Пример для вывода

![из справки Block.io - https://block.io/api](../.gitbook/assets/image%20%288%29.png)

{% hint style="success" %}
Мы знаем о параметрах: `to_addresses` и `from_labels` из [справки block.io](https://block.io/api/). Смотрите метод там "Withdraw From Labels(Вывод из меток)"
{% endhint %}

Переведите монету из кошелька Block.io с надписью «default» на любой кошелек:

```javascript
Libs.BlockIO.Bitcoin.withdrawFromLabels(
    { to_addresses:"3KyRZdjJCpHjHfnjaELvrDpNAxUE3D1NK4",
      from_labels:"default",
      amounts:0.002,
      onSuccess: "/onwithdraw", onError: "/onerror"
});
```

onWithdraw команда:

```javascript
Bot.sendMessage(inspect(options))
```

### Ошибка передачи

### Вы можете показать сообщение об ошибке из Block.io пользователю



```javascript
Bot.sendMessage("Ошибка");

if(options&&options.data){
  Bot.sendMessage(options.data.error_message);
}

```



## Как...

### ...открыть новый BTC адрес для каждого пользователя \(чата\)?

возможный BJS для команды / newAddress:

```javascript
Libs.BlockIO.Bitcoin.getNewAddress(
    {
      label: "chat" + chat.chatid,
      onSuccess: "/onNewAddress",
      onError: "/onError",
} );
```

команд /onNewAddress:

```javascript
Bot.sendMessage(inspect(options));
Bot.sendMessage("Создан: " + options.address);
```

команда /onError:

```javascript
Bot.sendMessage("Произошла ошипка при создании кошелька");
```



### Как я могу проверить любой платеж пользователя на [block.io] (http://block.io/), если я знаю его адрес в биткойнах и оплата будет 0,0001 биткойн?

Похоже, вам нужно получать платежи на один собственный кошелек от нескольких пользователей.

Это плохая практика

1. Пользователь должен сказать свой кошелек до оплаты. Потому что после оплаты любой может увидеть любые адреса в блокчейне
2. Пользователь может сделать ошибку со своим адресным кошельком
3. Пользователь может сделать ошибку с суммой

{% hint style="danger" %}
Таким образом, пользователи будут иметь некорректные операции. Вы должны проверить их в ручном режиме. Вам будет очень трудно проверить неверные транзакции в блокчейне.
{% endhint %}

Более эффективная практика - генерировать новый адрес дохода для каждого платежа. Это более безопасно.