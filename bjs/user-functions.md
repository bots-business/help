# User functions

| Function                              | Description                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `User.setProperty(name, value, type)` | <p>Set property with name for user. Name is case sensitive.</p><p></p><p><code>User.setProperty("city", "London", "string")</code></p><p></p><p>Type can be: <code>integer, float, string, text, json, datetime</code></p>                                                                                                                                                                           |
| `User.getProperty(name)`              | <p>Read property with name. Name is case sensitive.</p><p></p><p><code>User.getProperty("city")</code><br><br>can get property with default value for non exist property:</p><p><code>User.getProperty("city", "London")</code></p><p><code></code></p><p>can get property of another bot:</p><p><code>  </code>Bot.getProperty({ name: "propName", other_bot_id:   <code>OTHER_BOT_ID })</code></p> |
| `User.addToGroup(group_name)`         | <p>Add user to group with group_name</p><p></p><p><code>User.addToGroup("guests")</code></p>                                                                                                                                                                                                                                                                                                         |
| `User.getGroup()`                     | Get current user's group                                                                                                                                                                                                                                                                                                                                                                             |
| `User.removeGroup()`                  | Remove user from current group                                                                                                                                                                                                                                                                                                                                                                       |

**Access to property** in answer:

> You can also use the properties in the command's answer. For example, you can do this with the / hello command:`Hello, <UserRole>!`

in BJS:

> And you can use it in `Bot.sendMessage("Hello, <UserRole>")`
