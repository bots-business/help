# Automatic importing on Git push

You can make automatic bot deploying on git push. 

This possible with [Webhooks](https://help.bots.business/libs/webhooks-lib)

## Setup

command `/setupGit`

```javascript
url = Libs.Webhooks.getUrlFor(
   { command: "onGitPush", user_id: user.id }
)

Bot.sendMessage(url);
```

execute `/setupGit` copy url and go to Github.com &gt; your repository -&gt; Settings -&gt; Webhooks

Past copied url as Payload URL

![](../.gitbook/assets/image%20%2835%29.png)

Make like this:

![](../.gitbook/assets/image%20%2840%29.png)

Go to App - create command `onGitPush`

```javascript
Bot.sendMessage("Start code importing...");

// Bot.exportGit also possible
Bot.importGit({
  branch: "master",
  success: "onGitImportCompleted"
})
```

command `onGitImportCompleted`

just put to answer: "Git import completed"

{% hint style="warning" %}
Commands `onGitPush and onGitImportCompleted` must be in repository also. Because all commands will be deleted on git importing
{% endhint %}

{% hint style="danger" %}
Protect onGitPush command if you need this. Anybody can run it. 
{% endhint %}





