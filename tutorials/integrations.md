---
description: Practice outgoing HTTP, incoming webhooks and a Telegram Mini App form
  with complete callback flows and observable results.
cover: ../.gitbook/assets/cover-integrations.webp
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Integrations: HTTP, webhooks and Mini Apps

**Your result:** three working integration exercises in a test bot: handle an outgoing HTTP response, receive an incoming JSON webhook, and receive a selection from a Telegram Mini App form.

**Before you start:** know how to create and save BJS commands, use a private Telegram test chat, and read [BJS execution](../bjs/README.md#how-execution-works). For the last stage, you also need a separate HTTPS host for an HTML page. The BJS Web App endpoint returns JSON; it does not host the HTML form in this route.

{% stepper %}
{% step %}
### Handle an outgoing HTTP result

**Prepare:** reserve the three command names in [A complete GET flow](../bjs/http.md#a-complete-get-flow). If any name already exists, choose new names and update both callback references and their command names together.

**Do:** create the request command and both success and error commands from the HTTP guide. Keep them together; the result is handled by a later command, not returned inline from `HTTP.get`.

**Check:** run the request and confirm that the response handler sends the result described in the guide. Verify the saved callback names and read the Errors tab if it does not. To exercise a failure, temporarily use a controlled endpoint that you know fails, then restore the working URL; do not treat a manually invoked callback as a transport test.

**Next:** receive a request initiated by another service.
{% endstep %}
{% step %}
### Receive an incoming webhook

**Prepare:** install the `Webhooks` library (accessed as `Libs.Webhooks` in BJS) and follow the guide's context requirements. Use test data. The generated URL is sensitive; a demonstration callback is not provider authentication.

**Do:** follow [Receive an external webhook](../libraries/webhooks.md): generate the callback URL, create the receiving command, and send the guide's JSON request with an HTTP client.

**Check:** the receiving command sees the JSON body and returns the result described in the guide. Follow [Match the sender's protocol](../libraries/webhooks.md#match-the-senders-protocol) before connecting a real provider, especially for signatures, encoding, and retries.

**Next:** open a form inside Telegram.
{% endstep %}
{% step %}
### Open a Mini App form

**Prepare:** publish the guide's HTML on your own HTTPS host. Replace its example host with that real address. If the bot already has a `*` command, add the `web_app_data` branch to it and retain its other branches; do not replace your existing receiver.

**Do:** complete [Build a Telegram Mini App form](../guides/mini-app.md): publish the form, create `/open-form`, and receive its selection in `*`.

**Check:** use the reply-keyboard button in the bot's private Telegram chat, submit the form, and confirm the selection appears in the bot reply. A page opening in an ordinary browser alone does not prove the Telegram send-data flow. Use the full test checklist in the guide.

**Next:** choose another [integration](../guides/recipes.md#automation-and-services) or return to [Tutorials](README.md).
{% endstep %}
{% endstepper %}

{% hint style="info" %}
<img src="../.gitbook/assets/mel-02-help-menu-mobile.webp" alt="" width="64">

**Mel’s tip**

A request command and its callback are two parts of one flow. When you rename a callback command, change the request’s callback name too. Test the request from the beginning so the result arrives with the expected context.
{% endhint %}

## Finish the route

Keep a record of each trigger and observed reply. Outgoing HTTP, an incoming webhook, and Mini App data are separate paths; success in one does not confirm the other two. Review [safe command design](../bjs/security.md) before moving from test data to a live integration.
