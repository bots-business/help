# Migration from properties to list

For example you have such old code. Saving product in price list

```javascript
// we have array with products prices
var priceList = Bot.getProperty("priceList")
if(!priceList){ priceList = [] }

var curProduct = { name: 'Apple iPhone 25', price: 5100 }
global.push(priceList)

// we store all products in property preciList
Bot.setProperty("priceList", priceList, "json")
```

And we can get products from list now:

```javascript

Bot.sendMessage(priceList[0].name + ":" + priceList[0].price)

```

This code is good for small priceList. But is very bad for large count of products \(more then 200-500\). 

**So we need migrate it to Lists**

Saving product in price List:

```javascript
Bot.setProperty({
  name: 'Apple iPhone 25',
  value: 5100,
  type: 'float',
  list: 'priceList'  // bot list will be created if not exist
});
```

And we can get sorted top list now:

```javascript
let list = new List.new({ name: "priceList" })

let products = list.get();  // get first 100 (by default) products

Bot.sendMessage(products[0].name + ":" + products[0].price)
```

Get next 100 products:

```javascript
// get next 100 products:
let list = new List.new({ name: "priceList" })
list.page = 2;
let products = list.get();  // get first 100 (by default) products

Bot.sendMessage(products[0].name + ":" + products[0].price)
```

