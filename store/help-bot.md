# Help bot

![](../.gitbook/assets/image%20%2816%29.png)

In Bots.Business chat we have many generic questions. [This bot](https://telegram.me/BBHelpBot) can answer for thems.

Bot have only one command. This command is [Master command](https://help.bots.business/commands#how-to-execute-command-with-any-text-from-user-master-command) - \*.

### Code description

Bot receive all messages from chat with Master command.

So we do not need any notifications about new chat members and etc:

```javascript
if(!message){ return }
```

{% hint style="info" %}
Do you have big case for command execution? You can use return.  
{% endhint %}

Bot have keywords for searching in user's messages. Bot show answer and url on keywoard in message:

```javascript
list = [   { url: "status.bots.business", keywords: [ 'status' ],       answer: 'Seems do you need to know uptime status?' },   { keywords: [ '/start' ], answer: 'Please do not touch it here' },   { keywords: [ 'php ', ' php' ], answer: 'PHP? Really? I love BJS only' },   { keywords: [ 'hi!', 'hello' ], answer: 'Hey!' }]
```

Also admin can write anything and do not need any help. So we have key - answerToAdmin:

```javascript
let admin_tg_id = 519829299;...{ url: "status.bots.business", keywords: [ 'status' ], answerToAdmin: true }
```

Sometimes we need exact searhing:

```javascript
{ url: "help.bots.business", keywords: [ 'help' ], exact:true}
```

Message can be in aNyCAse. We need it only in lowercase:

```javascript
let stext = message.toLowerCase();
```

We use functions in the code. So code more simple:

```javascript
// search keyword in the message (stext)function haveAnyKeyword(item){  for(var ind in item.keywords){    // exact searhing    if(item.exact){      // exact searching      if(stext==item.keywords[ind]){ return true }      continue;    }    if(stext.indexOf(item.keywords[ind])>-1){ return true }  }}// build answerfunction getAnswerFor(item){  if((user.telegramid==admin_tg_id)&&(!item.answerToAdmin)){     // no any answer for admin     return;  }    let answer = item.answer;  if(!answer){ answer = "" }  if(item.url){    answer = answer + "\nhttp://" + item.url  }  return answer;}// just bust all keywords list function doSearch(){  let item;  let answer;  for(var ind in list){    item = list[ind];    if(haveAnyKeyword(item.keywords)){      return getAnswerFor(item);    }  }}
```

{% hint style="info" %}
Functions create your code more simple. It is good for it rewriting and improvement.
{% endhint %}

Perform searching. And if we get answer - send message:

```javascript
let answer = doSearch();if(answer){  Bot.sendMessage(answer, {is_reply: true});}
```

{% hint style="info" %}
In this command - we have only one Bot.sendMessage function!

It is good - code more simple.
{% endhint %}

