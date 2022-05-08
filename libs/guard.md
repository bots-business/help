# Guard

Gives access to individual commands only to admins.

Initial setup:

* Install Library
* create `/setup` command: code:

```javascript
Libs.Guard.setup();
```

You will have such admin panel:

![](<../.gitbook/assets/image (91).png>)



Add the commands you want to restrict access to to the "admins" folder (by default).

You can add/remove admins, change the folder for admin commands and add a command for unauthorized access attempts in the admin panel:&#x20;

`App > Bot > Admin Panels > Admin Guard.`
