# Guard

Gives access to individual commands only to admins.

Initial setup:

* Install Library
* Сreate `/setup` command:

```javascript
Libs.Guard.setup();
```

You will have such admin panel:

![](<../.gitbook/assets/image (91) (1) (1).png>)

* Create the @ command or add to an existing one the following code (at the very beginning):

```
if (!Libs.Guard.verifyAccess()) return;
```

Add the commands you want to restrict access to to the "admins" folder (by default).

You can add/remove admins, change the folder for admin commands and add a command for unauthorized access attempts in the admin panel:&#x20;

`App > Bot > Admin Panels > Guard.`

### Checking access

{% hint style="success" %}
Just put all your admin commands in one folder.

The check will be automatic.
{% endhint %}

For manuall checking mode use:

```javascript
if (Libs.Guard.isAdmin(user.id)){
   // admin
}else{
   // not admin
}
```
