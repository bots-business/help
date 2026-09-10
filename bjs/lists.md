---
description: Create BJS lists of properties or users, read pages of results, recount totals, and understand removal, membership, and migration behavior.
---

# Lists

A `List` groups stored properties or users so a command can load a page of results. Use it instead of repeatedly reading hundreds of separately named properties in one execution.

## Create and fill a property list

Create `/setup-catalog` and run it once on your test bot:

```javascript
var catalog = new List({ name: "catalog" });
if (!catalog.exist) { catalog.create(); }
Bot.setProp({
  name: "item_tea",
  value: { title: "Tea", price: 3 },
  type: "json",
  list: "catalog"
});
Bot.setProp({
  name: "item_coffee",
  value: { title: "Coffee", price: 4 },
  type: "json",
  list: "catalog"
});
Bot.sendMessage("Catalog prepared. Send /catalog to read it.");
```

Create `/catalog` as a separate command:

```javascript
var catalog = new List({ name: "catalog" });
if (!catalog.exist) {
  Bot.sendMessage("Create the catalog first.");
  return;
}
catalog.page = 1;
catalog.per_page = 10;
var items = catalog.get();
var lines = items.map(function (item) {
  return item.value.title + ": " + item.value.price;
});
Bot.sendMessage(lines.length ? lines.join("\n") : "The catalog is empty.", { parse_mode: null });
```

Creation and writes are actions. Do not create a list and expect a new `get()` in the same execution to act as a committed reload. Read it in the next command.

## Scope, pages, and results

`new List({name})` opens a bot list. `new List({name, user_id: user.id})` or `new List({name, user: user})` opens a user-scoped list. Guard against missing `user` before using the latter.

| Field or method | Meaning |
| --- | --- |
| `exist` | Whether the loaded list already exists |
| `id`, `name` | Internal list identity |
| `page` | Page number, starting at 1 |
| `per_page` | Page size; default 100; choose a smaller value for chat output |
| `get()` | Property records on the selected page |
| `search(text)` | Present in the wrapper, but its runtime search implementation is currently empty; do not use it to decide whether a match exists |
| `count`, `total_pages` | Stored/derived totals; may need recounting |

Each property record contains `name`, `value`, creation/update timestamps, and `user` when it belongs to a user. A property's name still identifies the property; attaching it to a list does not create an unrelated copy of the same property.

For a small page you have already fetched with `get()`, JavaScript filtering can narrow those records. It checks only that page; it is not a search across the whole list. Do not interpret the unimplemented `search()` result as proof that an item is absent.

The current property-list query returns records by internal property ID in ascending order. Although `order_by` and `order_ascending` fields exist, do not rely on them to sort property results by price or score. Sorting one fetched page in JavaScript is only a local page sort, not a global leaderboard.

## Recount list

Totals can lag behind changes. Recount deliberately, rather than before every read:

```javascript
// Command: /refresh-catalog
var catalog = new List({ name: "catalog" });
if (!catalog.exist) { return; }
if (catalog.isRecountNeeded()) {
  catalog.recount({ onComplete: "/catalog" });
} else {
  Bot.runCommand("/catalog");
}
```

`isRecountNeeded()` considers the last recount and its cost. `recount` may complete through background work; `onComplete` names the next command. Do not expect the JavaScript object's totals to refresh immediately after queuing a recount.

## Lists of users

Use an existing list and a known user record:

```javascript
// Command: /join-news
if (!user) { return; }
var readers = new List({ name: "news_readers" });
if (!readers.exist) { readers.create(); }
readers.addUser(user);
Bot.sendMessage("Added to the reader list.");
```

`getUsers()` returns paginated user records. `haveUser({id})` / `getUser({id})` returns the matching user record when present; it is not a strict boolean method. `rejectUser({id})` removes that membership, and `rejectAllUsers()` removes memberships from the list. These IDs are internal user IDs, not Telegram IDs.

## Remove data carefully

| Method | Current effect |
| --- | --- |
| `rejectProperty(name)` | Detach one property from the list, preserving the property |
| `removeProperty(name)` | Delete the named property |
| `remove()` | Remove the list record |
| `removeAll()` | Delete the list and its associated properties |
| `rejectAll()` | Currently deletes associated property records; do not use it as a safe bulk-detach operation |

When preserving data matters, detach individual properties with `rejectProperty`. Test deletion and migration on a disposable bot before applying them to valuable data.

## Migrate a JSON array into a List

Attaching an existing property to a list keeps it as **one property**. It does not turn an array stored inside that property into separate list entries. This recipe creates one JSON property per product and leaves the original array unchanged.

Use a disposable bot with the ordinary database for this exercise. Lists use database records; this recipe is not a memory-database migration. Reserve the new list name `catalog_v2` and property prefix `catalog-v2:` for this migration. Do not use them for unrelated data.

Create the three commands below with empty **Answer** and **Keyboard**, **Wait for answer** off, and Auto Retry empty. Follow [trusted administrator setup](security.md#restrict-a-command-to-a-trusted-telegram-user) and replace `YOUR_TELEGRAM_USER_ID` yourself in each command.

### Prepare a small source array

Create `/seed-legacy-catalog`. Run it only to create the demonstration source; for your own migration, use your existing source property and adapt the validated schema first.

```javascript
// Command: /seed-legacy-catalog
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
if (Bot.getProp("legacy_catalog", null) !== null) {
  Bot.sendMessage("Source already exists; left unchanged.");
  return;
}
Bot.setProp("legacy_catalog", [
  { id: "tea", title: "Tea", price: 3 },
  { id: "coffee", title: "Coffee", price: 4 }
], "json");
Bot.sendMessage("Source prepared. Send /migrate-catalog next.");
```

Each product has a stable unique `id`. Keep that ID when changing a title; do not use an array index, a timestamp, or a translated title as the destination key.

### Validate, then write the entries

Create `/migrate-catalog`:

```javascript
// Command: /migrate-catalog
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
const source = Bot.getProp("legacy_catalog");
const MAX_ITEMS = 20;
if (!Array.isArray(source) || source.length < 1 || source.length > MAX_ITEMS) {
  Bot.sendMessage("Expected an array with 1 to 20 products; nothing written.");
  return;
}
const ids = new Set();
for (const item of source) {
  const valid = item && typeof item === "object" && !Array.isArray(item) &&
    Object.keys(item).sort().join(",") === "id,price,title" &&
    typeof item.id === "string" && /^[a-z][a-z0-9_-]{0,31}$/.test(item.id) &&
    typeof item.title === "string" && item.title.trim().length > 0 && item.title.length <= 80 &&
    typeof item.price === "number" && Number.isFinite(item.price) &&
    item.price >= 0 && item.price <= 1000000;
  if (!valid || ids.has(item.id)) {
    Bot.sendMessage("Invalid product or duplicate ID; nothing written.");
    return;
  }
  ids.add(item.id);
}

// All source records have passed validation before any write is queued.
const destination = new List({ name: "catalog_v2" });
if (!destination.exist) { destination.create(); }
for (const item of source) {
  Bot.setProp({
    name: "catalog-v2:" + item.id,
    value: item,
    type: "json",
    list: "catalog_v2"
  });
}
Bot.sendMessage("Migration writes queued. Send /verify-catalog separately.");
```

The explicit `json` preserves each record's fields, including a price of zero. The first loop validates **every** record before the second loop queues writes. This bounded example accepts at most 20 products; it does not silently truncate a larger source.

Run it once at a time against an unchanged source. Repeating the same migration updates the same keys, so it does not create duplicate products. It is not a transaction: a backend failure can leave some entries written. Keep the source and rerun after resolving the error. Changing the source or removing its items requires a separate reconciliation plan; this command does not remove older destination entries.

### Verify in another execution

Create `/verify-catalog`:

```javascript
// Command: /verify-catalog
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
const source = Bot.getProp("legacy_catalog");
if (!Array.isArray(source) || source.length < 1 || source.length > 20) {
  Bot.sendMessage("The source is missing or outside this recipe's bounds.");
  return;
}
const destination = new List({ name: "catalog_v2" });
if (!destination.exist) {
  Bot.sendMessage("Run /migrate-catalog first.");
  return;
}
destination.page = 1;
destination.per_page = 21;
const records = destination.get();
const sourceIds = source.map(function (item) { return item && item.id; });
const matches = new Set(sourceIds).size === source.length &&
  records.length === source.length && source.every(function (item) {
  if (!item || typeof item.id !== "string") { return false; }
  const record = records.find(function (row) { return row.name === "catalog-v2:" + item.id; });
  return record && record.user === null && record.value &&
    record.value.id === item.id && record.value.title === item.title &&
    record.value.price === item.price;
});
if (!matches) {
  Bot.sendMessage("Verification failed. Keep the source and inspect the destination.");
  return;
}
Bot.sendMessage("Verified " + records.length + " products. Original legacy_catalog kept.");
```

With the demonstration source, expect `Verified 2 products. Original legacy_catalog kept.` Run `/migrate-catalog` and `/verify-catalog` again: the result should still be two products. The check fetches up to 21 records because the source limit is 20, so an extra destination entry is detected. It counts the fetched records rather than trusting a possibly stale list `count`.

After verification, update the consuming commands to read `new List({name: "catalog_v2"})` with `get()` and use each row's `value.title` and `value.price`. Keep a backup and retain `legacy_catalog` until every reader has been checked. For a larger collection, design batches with a saved cursor and the same stable IDs; do not merely increase an unbounded loop.

For missing records, check bot/user scope, name, page, and whether the write completed. Related: [properties](user-properties.md), [background work](background.md), and [BJS errors](errors.md).
