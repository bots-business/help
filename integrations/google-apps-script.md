---
description: Build a fixed-operation Apps Script endpoint for test data, understand current BJS transport limits, and review
  legacy GoogleApp and GoogleTableSync behavior.
---


# Connect a bot to a Google Sheet

An Apps Script web app can read or update spreadsheet data. The example below defines fixed operations and a shared-secret check, rather than accepting JavaScript code from a request. Use a disposable test sheet and test secret: the current direct BJS HTTP connection has redirect and HTTPS certificate-validation limitations, so this is an adapter example to evaluate, not a production-ready connector.

For turning a table into bot commands, use [CSV import](google-table-import.md) instead.

## Create a small spreadsheet endpoint

Create a test spreadsheet with a sheet named `Records`. In its Apps Script project, set **Script properties** named `BB_SHARED_SECRET` and `BB_SHEET_ID`. Use a new unpredictable secret and the spreadsheet's ID. Keep both configuration values out of the published script source.

Add this Apps Script code:

{% code title="Create a small spreadsheet endpoint · Example 1" overflow="wrap" %}
```javascript
function doPost(e) {
  let body;
  try { body = JSON.parse(e.postData.contents); }
  catch (error) { return reply({ ok: false, error: "invalid_json" }); }
  if (!body || typeof body !== "object" || Array.isArray(body)) {
    return reply({ ok: false, error: "invalid_request" });
  }
  const config = PropertiesService.getScriptProperties();
  const secret = config.getProperty("BB_SHARED_SECRET");
  if (!secret || body.secret !== secret) {
    return reply({ ok: false, error: "unauthorized" });
  }
  if (!isValidRequest(body)) { return reply({ ok: false, error: "invalid_request" }); }

  const lock = LockService.getScriptLock();
  if (!lock.tryLock(1000)) { return reply({ ok: false, error: "busy" }); }
  try {
    const sheet = SpreadsheetApp.openById(config.getProperty("BB_SHEET_ID"))
      .getSheetByName("Records");
    if (!sheet) { return reply({ ok: false, error: "sheet_not_found" }); }
    if (sheet.getLastRow() === 0) { sheet.appendRow(["id", "name"]); }
    const rows = sheet.getDataRange().getDisplayValues();
    const index = rows.findIndex(function (row, i) {
      return i > 0 && row[0] === body.id;
    });
    const result = body.action === "read"
      ? readRecord(rows, index)
      : saveRecord(sheet, index, body);
    return reply(result);
  } finally {
    lock.releaseLock();
  }
}

function reply(value) {
  return ContentService.createTextOutput(JSON.stringify(value))
    .setMimeType(ContentService.MimeType.JSON);
}

function isValidRequest(body) {
  return ["read", "save"].includes(body.action) &&
    typeof body.id === "string" && body.id.length > 0 && body.id.length <= 100;
}

function readRecord(rows, index) {
  const record = index < 0 ? null : { id: rows[index][0], name: rows[index][1] };
  return { ok: true, record: record };
}

function saveRecord(sheet, index, body) {
  if (typeof body.name !== "string" || body.name.length > 100 ||
      /^[=+@-]/.test(body.name) || /^[=+@-]/.test(body.id)) {
    return { ok: false, error: "invalid_name_or_id" };
  }
  const rowNumber = index < 0 ? sheet.getLastRow() + 1 : index + 1;
  const cells = sheet.getRange(rowNumber, 1, 1, 2);
  cells.setNumberFormat("@");
  cells.setValues([[body.id, body.name]]);
  return { ok: true, id: body.id };
}
```
{% endcode %}

Deploy it as a web app whose execution account has access to this test sheet. Bots.Business must be able to call the deployment without an interactive Google sign-in; access options depend on your Google account. This endpoint authenticates the body with the shared secret and accepts only its two fixed actions. Use the deployed `/exec` URL, not the editor or a development-only URL. [Apps Script web apps](https://developers.google.com/apps-script/guides/web).

## Evaluate a BJS request with test data

Google's ContentService redirects results to a one-time `script.googleusercontent.com` URL. The current BJS HTTP redirect handler repeats POST, body and headers on a redirect, including 302/303, instead of switching to GET. Consequently, `folow_redirects: true` alone does not establish that this adapter's result can be retrieved correctly. This direct request has not been validated against a deployed Apps Script endpoint. A result-retrieval failure also does not prove that the initial write failed. [ContentService redirects](https://developers.google.com/apps-script/guides/content#redirects).

The HTTP transport also does not validate HTTPS certificates. A production connector needs validated transport and correct redirect behavior before carrying sensitive data or credentials. Moving the endpoint behind a plain relay does not fix the unverified BJS-to-relay connection. See [HTTP transport limitations](../bjs/http.md#https-transport-limitation).

For an experiment with disposable data, store the deployed URL and test secret as bot properties `sheetEndpoint` and `sheetSecret` through an owner-only setup flow:

{% code title="Evaluate a BJS request with test data · Example 2" overflow="wrap" %}
```javascript
HTTP.post({
  url: Bot.getProp("sheetEndpoint"),
  headers: { "Content-Type": "application/json" },
  body: {
    secret: Bot.getProp("sheetSecret"),
    action: "save",
    id: "test-record-1",
    name: "Example"
  },
  folow_redirects: true,
  success: "/sheet-result",
  error: "/sheet-error"
});
```
{% endcode %}

The BJS parameter is spelled **`folow_redirects`** in the current HTTP contract. If the endpoint result is retrieved, handle it in `/sheet-result`:

{% code title="Evaluate a BJS request with test data · Example 3" overflow="wrap" %}
```javascript
let result;
try { result = JSON.parse(content); }
catch (error) { Bot.sendMessage("The sheet returned an unexpected response."); return; }
Bot.sendMessage(result && result.ok === true
  ? "Spreadsheet request completed."
  : "Spreadsheet request failed.");
```
{% endcode %}

In `/sheet-error`, send a short retry message. To read, send the same request with `action: "read"`, the same `id`, and no `name`. The reply contains `record` or `null`. Saving the same ID updates its row; use your own record IDs for separate orders or records.

Test with a non-sensitive sheet first. The script can return application errors in a successful HTTP response, so check `ok`. Review Apps Script quotas and execution logs if requests time out or permissions fail.

## Existing GoogleApp and GoogleTableSync libraries

The historical `GoogleApp` library exposes `setUrl(url)` and `run({ code: namedFunction, onRun, email, debug })`. It requires `Webhooks`, sends BJS context to the Apps Script endpoint, and receives the result through a webhook callback.

The matching public `GoogleAppSync.gs` executes code received in the request with `eval`, and its GET handler displays cached request/results. Do not deploy that connector unchanged as a public owner-executed endpoint for a sensitive account. Use fixed operations like the example above, together with validated transport and a reviewed data boundary. This is a limitation of that connector design, not a statement that Google Apps Script is unavailable.

`GoogleTableSync.sync` and `.read` depend on that GoogleApp connector. Their options include `tableID`, `sheetName`, `datas`, `index`, and `onRun`; a sync callback receives `newCount`/`updatedCount`, while read returns matching records. The checked implementation searches matching values across cells, rather than restricting the lookup to only the named index column, and skips falsy write values such as zero. Do not use it for records requiring exact-key matching or reliable zero-value updates without correcting that implementation.

## Migrate a read and write workflow

The old libraries still expose these methods. The example below replaces one small `GoogleTableSync` workflow with the fixed `read`/`save` operations above. It is an **evaluation path**, not a claim that the new HTTP connector is already a working replacement for your deployed integration. The old connector returns results through a separate webhook; the new adapter returns them in the HTTP response. Its redirect/transport limitations must be resolved and its response path verified before switching a real workflow.

### Map the existing contract

Inventory every `Libs.GoogleApp.run`, `Libs.GoogleTableSync.sync` and `.read` call, its input fields, callback commands and consumers. Record the current sheet headers and keep a copy of the sheet and bot commands. Do not send writes to both old and new connectors during the test.

| Existing setting or result | Mapping to this adapter |
| --- | --- |
| `GoogleApp.setUrl(url)` | New deployment URL in the bot's `sheetEndpoint` property; changing the old library URL does not convert its protocol |
| `tableID`, `sheetName` | Trusted Apps Script configuration: `BB_SHEET_ID` and the fixed `Records` sheet. The request cannot select an arbitrary spreadsheet |
| `index: "orderId"` | Map the record's `orderId` value to the new string `id`; it is matched only against the first column |
| `datas: [record1, record2, ...]` | One `save` request per mapped record; this example does not accept an array |
| Fields named by sheet headers | Map explicitly to the adapter's `id` and `name` columns; it does not automatically create columns |
| `sync(... onRun: "/onSync")` | `HTTP.post(... success: "/sheet-migrate-saved")`; parse `content`, check `ok` and the returned `id` |
| Sync callback `options.newCount` / `options.updatedCount` | Save reply `{ ok: true, id }`. It does not report whether the row was inserted or updated; do not fabricate the old counts |
| `read({ datas: [{ orderId: "order-demo-1" }], index: "orderId", ... })` | `action: "read", id: "order-demo-1"`, with no `name` |
| Read callback `options` is an array of found records | New response is `{ ok: true, record: { id, name } }`, or `record: null` for a missing ID. Parse `content` first |
| `email`, `debug` | No matching request options here; use Apps Script execution logs and the BJS error callback |
| `GoogleApp.run({ code: namedFunction, ... })` | Review what the function does and implement a named, fixed server operation. Arbitrary code and the full BJS context are not accepted by this adapter |

For example, a former record `{ orderId: "order-demo-1", customerName: "Example" }` maps to `{ id: "order-demo-1", name: "Example" }`. Keep IDs stable: recreating a record must use the same ID. Check for duplicate IDs before importing existing rows, because this adapter uses the first exact match.

If your actual rows include price, quantity, booleans or other fields, extend the endpoint with explicit columns, types and validation before migrating them. The two-column example is not a complete replacement for that schema. The old library could skip zero/false/empty writes; decide each field's intended value and clear/update semantics rather than reproducing that accidental behavior. The example reads display strings, so it also does not preserve arbitrary spreadsheet cell types.

### Save one mapped record, then read it back

1. Prepare the separate test `Records` sheet, Script properties and deployed `/exec` endpoint described above. In an owner-only setup command, set `Bot.setProp("sheetEndpoint", "YOUR_DEPLOYED_EXEC_URL")` and `Bot.setProp("sheetSecret", "YOUR_DISPOSABLE_TEST_SECRET")`, using the same secret as `BB_SHARED_SECRET`. Remove literals and the setup command afterward.
2. Obtain your own Telegram user ID through [trusted-user setup](../bjs/security.md#restrict-a-command-to-a-trusted-telegram-user). Set it in all five commands below. Keep **Answer** and **Keyboard** empty and **Wait for answer** off. Run this small migration test from your private chat.
3. Create `/sheet-migrate-save`:

{% code title="Save one mapped record, then read it back · Example 4" overflow="wrap" %}
```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID ||
    !chat || chat.chatid !== ADMIN_TELEGRAM_ID) { return; }
const endpoint = Bot.getProp("sheetEndpoint");
const secret = Bot.getProp("sheetSecret");
if (!endpoint || !secret) {
  Bot.sendMessage("Configure the test endpoint and secret first.");
  return;
}
const oldRecord = { orderId: "order-demo-1", customerName: "Example" };
HTTP.post({
  url: endpoint,
  headers: { "Content-Type": "application/json" },
  body: { secret: secret, action: "save", id: oldRecord.orderId, name: oldRecord.customerName },
  folow_redirects: true,
  success: "/sheet-migrate-saved",
  error: "/sheet-migrate-error"
});
```
{% endcode %}

4. Create `/sheet-migrate-saved`. This receives an HTTP response in `content`, unlike the old `/onSync` webhook command's `options`:

{% code title="Save one mapped record, then read it back · Example 5" overflow="wrap" %}
```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
let result;
try { result = JSON.parse(content); }
catch (error) {
  Bot.sendMessage("No readable save result. Inspect the test sheet before retrying.");
  return;
}
if (!result || result.ok !== true || result.id !== "order-demo-1") {
  Bot.sendMessage("Save was not confirmed. Inspect the endpoint and test sheet.");
  return;
}
Bot.sendMessage("Save acknowledged. Send /sheet-migrate-read to verify the stored record.");
```
{% endcode %}

5. Create `/sheet-migrate-read`:

{% code title="Save one mapped record, then read it back · Example 6" overflow="wrap" %}
```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID ||
    !chat || chat.chatid !== ADMIN_TELEGRAM_ID) { return; }
const endpoint = Bot.getProp("sheetEndpoint");
const secret = Bot.getProp("sheetSecret");
if (!endpoint || !secret) {
  Bot.sendMessage("Configure the test endpoint and secret first.");
  return;
}
HTTP.post({
  url: endpoint,
  headers: { "Content-Type": "application/json" },
  body: { secret: secret, action: "read", id: "order-demo-1" },
  folow_redirects: true,
  success: "/sheet-migrate-read-result",
  error: "/sheet-migrate-error"
});
```
{% endcode %}

6. Create `/sheet-migrate-read-result`:

{% code title="Save one mapped record, then read it back · Example 7" overflow="wrap" %}
```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
let result;
try { result = JSON.parse(content); }
catch (error) { Bot.sendMessage("The read response was not valid JSON."); return; }
if (!result || result.ok !== true || !Object.prototype.hasOwnProperty.call(result, "record")) {
  Bot.sendMessage("Read failed or returned an unexpected shape.");
  return;
}
if (result.record === null) { Bot.sendMessage("No record found for the requested ID."); return; }
if (!result.record || result.record.id !== "order-demo-1" || result.record.name !== "Example") {
  Bot.sendMessage("Read-back differs from the mapped record. Inspect the test sheet.");
  return;
}
Bot.sendMessage("Verified order-demo-1: Example.");
```
{% endcode %}

7. Create `/sheet-migrate-error`:

{% code title="Save one mapped record, then read it back · Example 8" overflow="wrap" %}
```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
Bot.sendMessage("Spreadsheet transport failed. A write may still have happened; inspect the test sheet before retrying.");
```
{% endcode %}

Send `/sheet-migrate-save`, then `/sheet-migrate-read`. If the request and response paths work, expect a save acknowledgement and then `Verified order-demo-1: Example.` Independently check that the sheet has headers `id`, `name` and exactly one matching row. Save the same ID again and confirm it updates that row instead of appending another. Test a different name in both the save fixture and expected read-back, then test a missing ID by changing only the read request's ID: the endpoint should return `record: null`.

Also test a wrong secret, invalid input and an untrusted Telegram account. An HTTP success alone is insufficient: an application failure has `ok: false`. If the sheet changes but the callback cannot read the response, stop the migration test and resolve the response path; do not report the connector as verified.

### Move the remaining callers

After this evaluation succeeds and the transport limitation is resolved, extend the schema for your real records and verify representative values, duplicate keys and missing records. For several records, send a bounded sequence of requests and checkpoint each ID only after its response and read-back have been verified. Resume from that checkpoint after a failure; do not treat one successful save as success for an entire former `datas` array.

Update old callback consumers explicitly. If a caller still needs an array, collect verified `record` values by requested ID and deliberately handle `null`; if it needs insert/update counts, add that contract to the endpoint rather than inventing counts in the bot. Switch one caller at a time. Retire old library calls and the old public deployment only after remaining users and callback consumers have migrated; keep the original data until the new reads are checked. [HTTP](../bjs/http.md) explains request and response contexts.
