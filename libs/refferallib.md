---
description: Используйте эту библиотеку для отслеживания рефералов.
---

# RefferalLib

Демо бот: [https://telegram.me/DemoReferalTrackingBot?start=FromLibPage](https://telegram.me/DemoReferalTrackingBot?start=FromLibPage)

## Введение

Основная функция - **отслежка**. Предпочитается вызывать на /start:

`Libs.ReferralLib.currentUser.track(trackOptions);`

params(параметр) `trackOptions` - это оbject(объект) с функциями обратного вызова для:

<table>
  <thead>
    <tr>
      <th style="text-align:left">функция обратного вызова</th>
      <th style="text-align:left">описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">onTouchOwnLink()</td>
      <td style="text-align:left">юзер пришел нажал на свою ссылку</td>
    </tr>
    <tr>
      <td style="text-align:left">onAlreadyAttracted()</td>
      <td style="text-align:left">пользователь уже был привлечен</td>
    </tr>
    <tr>
      <td style="text-align:left">onAttracted()</td>
      <td style="text-align:left">пользователь был привлечен с канала</td>
    </tr>
    <tr>
      <td style="text-align:left">onAtractedByUser(refUser)</td>
      <td style="text-align:left">
        <p> пользователь был привлечен другим пользователем refUser - это общие данные пользователя (поля:
          nickname, first_name и др.)</p>
        <p></p>
        <p>Также есть поле chatId с идентификатором чата для этого пользователя.</p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
Смотрите детали в @[DemoReferalTrackingBot](https://telegram.me/DemoReferalTrackingBot?start=FromLibPage)
{% endhint %}

## Функции



## Получить реферальную ссылку для текущего пользователя

`Libs.ReferralLib.currentUser.getRefLink(bot.name);` 

### 

### Получить пригласителя для текущего пользователя

`Libs.ReferralLib.currentUser.attractedByUser()` 

возвращает информацию пользователя \(с chatId\) 



### Получить привлеченный канал для текущего пользователя

`Libs.ReferralLib.currentUser.attractedByChannel()` 

выводит канал, который привлек текущего пользователя



### Получить refList(лист рефералов)

`Libs.ReferralLib.currentUser.refList.get();` 

возвращает лист с привлеченными пользователями



### Очистить refList

`Libs.ReferralLib.currentUser.refList.clear();`

### 

### Получить топ лист рефералов

`Libs.ReferralLib.topList.get(45)`


выводит первых 45 пользователей, сортируя по количеству рефералов


### Очистить топ лист рефералов

`Libs.ReferralLib.topList.clear()`

## Как...	

**Q: Как дать бонус пользователю за привлеченного друга? **

**Ответ:**

Вы можете использовать [ResourcesLib](https://help.bots.business/libs/resourceslib) для этого.

при /start

```javascript
function doAttracted(refUser){
  // доступ к Бонус Ресурсу refUser
  let refUserBonus = Libs.ResourcesLib.anotherUserRes("money", refUser.telegramid);
  refUserBonus.add(100);  // добавить 100 бонуса за друга
}

Libs.ReferralLib.currentUser.track({
   doAtractedByUser: doAttracted
});
```



**Q: Как дать рефереру 5% от депозита реферального пользователя?**

**Ответ:**

1. Вам, для начала, надо установить [отслеживание](https://help.bots.business/libs/refferallib#getting-started)
2. Теперь нужно воспользоваться [ResourcesLib](https://help.bots.business/libs/resourceslib)
3. На установленос счете пользователя:

```javascript
let res = Libs.ResourcesLib.userRes("money");
let referrer = Libs.ReferralLib.currentUser.AttractedByUser();

// если недавний пользователь был привлеченным рефералом
if(referrer){
   let referrerRes = Libs.ResourcesLib.anotherUserRes(
       "money", referrer.telegramid);
   
   let amount = res.value * 0.05; // it is 5%
   referrerRes.takeFromAnother(res, amount);
}
```

{% hint style="info" %}
В этом примере мы используем userRes. Также возможно использование chatRes. См. [ResourcesLib] (https://help.bots.business/libs/resourceslib) для получения подробной информации.
{% endhint %}

