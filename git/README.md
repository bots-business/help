# Git

![](<../.gitbook/assets/image (93).png>)

## Git support

* Bot exporting to external Git repository. For example, Github
* Bot importing from external Git repository

## Requirements

#### 1. Need set Git repository on bot

Go to Bot->Edit-> show Advanced&#x20;

![](<../.gitbook/assets/image (2).png>)

#### 2. Need set Deploy Key on external repository

{% hint style="success" %}
You do not need Deploy Key for read access of public repository&#x20;
{% endhint %}

Go to Bot-> Sync in menu, then Git Sync -> Deploy Key show

![](<../.gitbook/assets/image (47).png>)

**Copy Deploy Key**

Now you need set this Deploy Key on external repository.

**Github**

Please see [this](https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys). For Git exporting also need "write" access.

#### 3. Now you can make exporting or importing with Git
