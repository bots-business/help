# Automatic importing on Git push

ou can make automatic bot deploying on git push.&#x20;

This possible with [Webhooks](https://help.bots.business/libs/webhooks-lib). Make install for this lib.

## Setup

command `/setupGit`

```javascript
var url = Libs.Webhooks.getUrlFor(
   { command: "onGitPush", user_id: user.id }
)

Api.sendMessage({
  text: "Github webhook: " +
     "\n<pre>" + url + "</pre>",
  parse_mode: "html",
  disable_web_page_preview: true
})

Bot.sendMessage(url);
```

execute `/setupGit` copy url and go to Github.com > your repository -> Settings -> Webhooks. Press button "Add webhook"

Past copied url as Payload URL

![](<../.gitbook/assets/image (87).png>)

Make like this:

![](<../.gitbook/assets/image (86).png>)

Go to App - create command `onGitPush`

```javascript
Bot.sendMessage("Start code importing...");

// Bot.exportGit also possible
Bot.importGit({
  branch: "master", // it is master branch
  success: "onGitImportCompleted"
})
```

command `onGitImportCompleted`

just put to answer: "Git import completed"

{% hint style="warning" %}
Commands `onGitPush and onGitImportCompleted`must be in repository also. Because all commands will be deleted on git importing
{% endhint %}

{% hint style="danger" %}
Protect onGitPush command if you need this. Anybody can run it.&#x20;
{% endhint %}



