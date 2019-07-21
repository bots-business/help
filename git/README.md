# Git

![](../.gitbook/assets/image%20%2817%29.png)

## Git support

* bot exporting to external Git repository \(for Github, for example\)
* bot importing from external Git repository

## Requirements

#### 1. Need set Git repository on bot

Go to Bot-&gt;Edit-&gt; show Advanced 

![](../.gitbook/assets/image%20%289%29.png)

#### 2. Need set Deploy Key on external repository

{% hint style="success" %}
You do not need Deploy Key for read access of public repository 
{% endhint %}

Go to Bot-&gt; Sync in menu, then Git Sync -&gt; Deploy Key show

![](../.gitbook/assets/image%20%2838%29.png)

**Copy Deploy Key**

Now you need set this Deploy Key on external repository.

**Github**

Please see [this](https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys). For Git exporting also need "write" access.

#### 3. Now you can make exporting or importing with Git

