# BlockIOBot

This bot is demo for integration with [Block.io](https://block.io). 

### See also related [Lib](https://help.bots.business/libs/blockio)

### With Block.io you can:

* create new wallets: Bitcoin, Dogecoin, Litecoin
* look transactions
* accept payments
* make withdraws
* etc

### Bot menus

Main menu on /start command:

![](../.gitbook/assets/image%20%2850%29.png)

Bot addresses menu on aliase "Bot addresses":

![](../.gitbook/assets/image%20%2815%29.png)

  
Withdrawals menu:

![](../.gitbook/assets/image%20%2847%29.png)

Tools menu:

![](../.gitbook/assets/image%20%2826%29.png)

### How it is works?

Bot use BlockIo lib. 

Typical code for command `/getXXX` is:

```java
Libs.BlockIO.Bitcoin.getXXX(    { onSuccess: "/onGetXXX", onError: "/onerror" });
```

`getXXX` - it is API methods from [https://block.io/api/simple](https://block.io/api/simple/)

Also we have `onSuccess` and `onError` commands.

All `onSuccess` command have name`/onGetXXX`

Bot have only one `onError` command: `/onerror`:

```javascript
Bot.sendMessage("Error");if(options&&options.data){  // in options we have error message from Block.io  // just send it  Bot.sendMessage(options.data.error_message);}
```



#### For example - command for address validation

Command name is:

```javascript
Libs.BlockIO.Bitcoin.isValidAddress(  { onSuccess: "/onvalidate",  onError: "/onerror",  address: message });
```

We also have address in message variables: command have value "wait for answer" from user.

Command: /onvalidate

We just send response:

```javascript
// we have json response from Block.io in options Bot.sendMessage(inspect(options));
```



### Getting addresses

Command: `/getMyAddresses`

```javascript
Libs.BlockIO.Bitcoin.getMyAddresses(  { onSuccess: "/ongetmyadresses", onError: "/onerror" });
```

Command: `"/onGetMyAdresses"`

```javascript
// Block.io response in options let wallets = options;Bot.sendMessage("Network: " + wallets.network);let addresses = wallets.addresses;let answer = "*Yours wallets:*\n"let counter = 0;// we have several addresses.for(let ind in addresses){  if(counter>10){ break } // no more then 10 addresses  counter+=1;  answer= answer + "#️⃣ `" +  addresses[ind].address + "`" +      "\n  🏷️Label: `" +                addresses[ind].label.split("_").join("") + "`" +      "\n  💰balance: `" +                addresses[ind].available_balance + "`" +      "\n  ⏳pending received balance: " +                addresses[ind].pending_received_balance +      "\n  ❌Archive: /archive" +                addresses[ind].label +      "\n\n"}Bot.sendMessage(answer);
```

### Transactions. Income and outgoing transactions

For outgoing transactions:

```javascript
Libs.BlockIO.Bitcoin.getTransactions(    { type: "sent",     onSuccess: "/onGetOutTransactions", onError: "/onerror" });
```

For income transactions:

```javascript
Libs.BlockIO.Bitcoin.getTransactions(    { type: "received",     onSuccess: "/onGetTransactions", onError: "/onerror" });
```

`/onGetOutTransactions` and `/onGetTransactions` command - is simular:

{% tabs %}
{% tab title="/onGetOutTransactions" %}
```javascript
let transactions = options;let answer = "";answer+= "Network: " + transactions.network;function parseOutcoming(tx){  let sended = tx.amounts_sent;   if(!sended){ return "" }  let result = ""  for(let ind in sended){    result+= "\n  📥recipient: `" + sended[ind].recipient + "`" +             "\n  💰amount: `" + sended[ind].amount + "`";  }  if(result==""){ return "" }    result+="\n  ▪senders: "  for(let ind in tx.senders){     result+= "`" + tx.senders[ind] + "` ";  }    return result;}let tx, time;for(let ind in transactions.txs){  tx = transactions.txs[ind];  time = new Date(tx.time*1000);  time = time.toLocaleString()    answer+= "\n\nTXID:`" + tx.txid + "`";  answer+= "\n  ⌚time: `" + time + "`";  answer+= "\n  🔢confirmations: " + tx.confirmations;    answer+= parseOutcoming(tx)}Bot.sendMessage(answer);
```
{% endtab %}

{% tab title="/onGetTransactions" %}
```javascript
let transactions = options;let answer = "";answer+= "Network: " + transactions.network;function parseIncoming(tx){  let received = tx.amounts_received;   if(!received){ return "" }  let result = ""  for(let ind in received){    result+= "\n  📥recipient: `" + received[ind].recipient + "`" +             "\n  💰amount: `" + received[ind].amount + "`";  }  if(result==""){ return "" }    result+="\n  ▪senders: "  for(let ind in tx.senders){     result+= "`" + tx.senders[ind] + "` ";  }    return result;}let tx, time;for(let ind in transactions.txs){  tx = transactions.txs[ind];  time = new Date(tx.time*1000);  time = time.toLocaleString()    answer+= "\n\nTXID:`" + tx.txid + "`";  answer+= "\n  ⌚time: `" + time + "`";  answer+= "\n  🔢confirmations: " + tx.confirmations;    answer+= parseIncoming(tx)}Bot.sendMessage(answer);
```
{% endtab %}
{% endtabs %}



### Master command "\*" for address actions

Bot need command for addresses archiving.

![](../.gitbook/assets/image%20%2827%29.png)

We need command /archiveLabel, where Label is label for address

So we have master command "\*" with BJS. It process all "/archiveXXX" commands:

```javascript
if(message.substring(0, 8)=="/archive"){   let arr = message.split("/archive");   let label = arr[1];   Libs.BlockIO.Bitcoin.archiveAddresses(      { onSuccess: "/onarchived", onError: "/onerror", labels:label }   );}
```





