# Admin Panel

\*\*\*\*

**You can create a custom admin panel.**

* make custom data fields: numeric, text, checkbox, password
* data fields will be accessible in BJS
* admin panels can be created via BJS
* bot run customized command on field saving
* supports severals panels with titles and differents fields

**Benefits:**

* making options for saving any api keys, secure and unsecure data
* can run any BJS logic from panel. For example: it will be possible create text field with button "Send this message to all chats" from App.
* make any quick statistic and information. Bot dashboards and etc

![](../.gitbook/assets/image%20%2829%29.png)

## Methods

### Define new Admin Panel

Admin Panel - this is a combination of several panels. Each panel have title, icon, description and one or more fields:

For adding panel:

`AdminPanel.setPanel({ panel_name: PANEL_NAME, data: PANEL_OPTIONS });`

PANEL\_OPTIONS - it is JSON option for this panel

**Example**

![](../.gitbook/assets/image%20%2850%29.png)

```javascript
var panel = {
  // Pabel title
  title: "Admin Information",
  description: "Please fill here your admin id",
  // order index
  index: 0,
  icon: "key",
  // save button title - default "SAVE"
  button_title: "SAVE",
  // command called on saving
  // not necessary
  /* on_saving:{
     command: "/on-saving",
     // if you need user
     user_id: user_id // Get it via Bot.sendMessage(user.id)
  },
  */
  
  // Fields for this Panel
  // here 1 field only
  fields: [
    {
      name: "ADMIN_ID",
      title: "Admin ID",
      description: "you can get your admin_id with BJS Bot.sendMessage(user.id)",
      type: "string",
      placeholder: "your admin id",
      // value: 100
    }
    // another fields here
    // if needed
    // ...
  ]
}

AdminPanel.setPanel({
  panel_name: "AdminInfo",
  data: panel
  // force: true // default false - save fields values
});
```

#### Force

Default is false. All old values for the fields are retained.

If true - all old values for fields are reassigned.

#### Fields

It is array of fields. One panel can have several fields. It is also possible panel without any field.

Fields can have name, value, title, description, type, placeholder and icon

#### Field type

| Type | Description |
| :--- | :--- |
| checkbox | Text input |
| integer | Text input |
| float | Text input |
| string | Text input |
| password | Password input |
| text | Text field |

### 

### Getting field value from Panel

{% hint style="success" %}
Use this method for getting one value from panel
{% endhint %}

```javascript
var admin_id = AdminPanel.getFieldValue({
  panel_name: "AdminInfo", // panel name
  field_name: "ADMIN_ID" // field name
})

Bot.sendMessage(admin_id)
```

### Getting all fields values from Panel

{% hint style="success" %}
Use this method for getting several/all values from panel
{% endhint %}

```javascript
var values = AdminPanel.getPanelValues("AdminInfo");
Bot.inspect(values);
// will be like:
// { ADMIN_ID: 100 }
```

###  

### Getting panel data

```javascript
var panel = AdminPanel.getPanel("AdminInfo")
Bot.inspect(panel);

// can modify panel
// panel.fields[0].value = 1000
// panel.fields[0].tite = "my admin id"
// AdminPanel.setPanel("AdminInfo", panel);
```

### 

### Getting panel field data

```javascript
var panel_field = AdminPanel.getPanelField({
  panel_name: "AdminInfo", // panel name
  field_name: "ADMIN_ID" // field name
})

Bot.inspect(panel_field);
```

### Icons

You can use all icons from [https://ionicons.com](https://ionicons.com/)



## Good practices

{% hint style="success" %}
Use admin panels to create a **configuration**
{% endhint %}

Define Admin Panels in `/config` command with `AdminPanel.setPanel` method.

Then use `AdminPanel.getPanelValue` method for getting any field's value



{% hint style="success" %}
Use admin panels to create a bot **dashboard**
{% endhint %}

Panel without fields can dispay any informations. Use this.



{% hint style="success" %}
Use admin panels to create admin reactions
{% endhint %}

You can launch any bot command on panel saving. It is good for making any admin command execution. Use `on_saving`: 

```javascript
var panel = {
  // Pabel title
  title: "Call secure command",
  description: "It is secure command",
  // order index
  index: 0,
  icon: "key",
  // save button title - default "SAVE"
  button_title: "RUN",
  // command called on saving
  // not necessary
  on_saving: {
     command: "/secure-command",
     // if you need user
     user_id: user_id // Get it via Bot.sendMessage(user.id)
  }
}

AdminPanel.setPanel("SecureCommand", panel);
```

