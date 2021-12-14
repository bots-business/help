# Lists

Use Lists to store large amounts of data. List is a quick way to [get](./#getting-data) and [search](./#searching) data. Also list is preferred way for organize [statistics](./#statistics).

List - it is a collection of **properties** or **users**.

Examples:

* history (orders history, transactions, payments history, locations history, etc)
* large price lists
* table of cities with population
* results for [Inline Bot](../inline-bot.md)
* referrals list

{% hint style="danger" %}
**Do not use JSON property** to collect a large array. This is a very bad practice.&#x20;

**"Execution timeout"** error - it is typical error if you use JSON prop for large data.

Use a List.
{% endhint %}

## List initialization

Before using the list (new or existing), you need to initialize it.

List can be global for Bot:

```javascript
let list = new List({ name: "MyList" })

Bot.inspect(list.exist) // false for new List, true for already exist list
```

or can be personal for user:

```javascript
let list = new List({ name: "MyList", user: user })

// or for user id
let list = new List({ name: "MyList", user_id: user.id })
```

After it you can perform another list methods

## Create new list

```javascript
// for list saving
if(!list.exist){
  list.create()
}
```

## Remove list

```javascript
list.remove();
```

## Recount list

Recalculate all statistical data in list.

Can spent 5-30 seconds and more for big list. So you need perform this task on background once in week/day or hour.

```javascript
list.recount({
    // this command will be runned after recount
    // onComplete: 'onCompleteListRecount'
});
```

## Calculate the amount of all props and users

Need [recount](./#recount-list) before

```javascript
list.total;  // integer value
```

## Properties management

### Add bot property to bot list

Bot property can be added to List for bot

{% hint style="success" %}
Example. Bot can have price list.
{% endhint %}

```javascript
Bot.setProperty({
  name: 'product1',
  value: 'Nano Cloud',
  type: 'string',
  list: 'PaidPlans'  // bot list will be created if not exist
});
```

### Add user property to user list

User property can be added to List for user&#x20;

{% hint style="success" %}
Example. User can have Orders list&#x20;
{% endhint %}

```javascript
User.setProperty({
  name: "order125",
  value: { product_id: "product1", price: 28 },
  type: "json",
  list: "Orders"  // bot list will be created if not exist
});
```

### Add property to bot list

User property can be added to List for bot

{% hint style="success" %}
Example. Bot can have top customers list.
{% endhint %}

```javascript
let list = new List({ name: "TopCustomers" })
if(!list.exist){ list.create() }

User.setProperty({
  name: "customer" + user.id,
  value: 20, // total income in USD
  type: "float",
  list: list
});
```



### Reject property

Reject property from list without destroying by prop name

```javascript
list.rejectProperty("stringProp");
```

### Remove property

Reject property from list and destroy it by prop name

```javascript
list.removeProperty("stringProp");
```

### Reject all properties (and all users)

Reject all properties from list

{% hint style="warning" %}
this method reject all properties and all users from list
{% endhint %}

```javascript
list.rejectAll();
```

### Remove all properties (and reject all users)

Remove all properties from list

{% hint style="warning" %}
this method remove all properties and reject all users from list
{% endhint %}

```javascript
list.rejectAll();
```

## Users management

### Add user

```javascript
list.addUser(user);

// or if you have id only
// list.addUser({ id: user.id });
```

### Reject user

Reject user from list

```javascript
list.rejectUser(user);
// or by id
list.rejectUser({ id: user.id });
```

### Reject all users

Reject all users from list

```javascript
list.rejectAllUsers();
```

## Getting data

### Getting props from list

```javascript
let props = list.get();
Bot.inspect(props[0])

/* result will be like:
  {
    "name": "stringProp",
    "value": "Bad Apple",
    "user": {
      "id": null
    },
    "created_at": "2020-11-09T01:46:52.270Z",
    "updated_at": "2020-11-09T01:46:52.270Z"
  }
*/
```

### Getting users from list

```javascript
let users = list.getUsers();
Bot.inspect(users[0])
```

### Paginating

We have pages for data. Data is given page by page from the first page.

One page have 100 items by default.

```javascript
// page 1
let props = list.get();
let users = list.getUsers();

// page 2
list.page = 2
props = list.get();  // getting props from page 2
users = list.getUsers(); // getting users from page 2

// it is possible to change default per page
list.per_page = 10 // 100 by default

// total pages
let total_pages = list.total_pages;
```

### Ordering

Default order is ascending by id (prop's id)

```javascript
// default order is ascending by id
let props = list.get();
props[0] // oldest prop in list

// Change order to descending:
list.order_ascending = false;
props[0] // newest prop in list
```

Ordering method can be changed from id (default) to other method:

```
list.order_by = "integer_value";

// list.order_by = "user_id";
// list.order_by = "size";
// etc - see table

list.order_ascending = true;
props[0] // prop with lowest value
```

| list.ordering\_by | Ordering props by                               |
| ----------------- | ----------------------------------------------- |
| user\_id          | order by user\_id                               |
| name              | order by name                                   |
| integer\_value    | order by integer value                          |
| float\_value      | order by float value                            |
| datetime\_value   | order by datetime value                         |
| boolean\_value    | order by boolean value                          |
| string\_value     | order by string value                           |
| text\_value       | order by text value                             |
| value\_type       | ordering by value type (integer, float and etc) |
| size              | order by value size                             |

## Searching

### Searching for text props

Searching is available for list's properties. (String and text props)

```javascript
// search prop with text value "Apple"
// case sensetive by default
let props = list.search("Apple");

// case insensentive
list.case_sensitive = false;
// search props with text value "Apple", "apple", "aPpLe" and etc
props = list.search("Apple");

// search props started with text "apple"
props = list.search("%Apple");

// search props ended with text "apple"
props = list.search("Apple%");

// search props contains text "apple"
props = list.search("%Apple%");


```

* Percent sign ( `%`) matches any sequence of zero or more characters.
* Underscore sign ( `_`)  matches any single character.

{% hint style="info" %}
Ordering and paginating also works with searching

```javascript
list.page = 2;
list.order_ascending = false;
let props = list.search("Apple");
```
{% endhint %}

## User searching

### Getting user by id from list

```javascript
// getting user from list if exist
list.getUser({ id: user.id })
```

### Checking the user's existence in the list

```javascript
list.haveUser(user)

// or by id:
list.haveUser({ id: user.id })
```

## Statistics

Is available for integer and float props

Need [recount](./#recount-list) before

```javascript
list.count  // total props + users count 
list.total_value  // sum of all props
list.average_value  // average of all props 
list.max_value
list.min_value
```

