# User functions

<table>
  <thead>
    <tr>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>User.setProperty(name, value, type)</code>
      </td>
      <td style="text-align:left">
        <p>Set property with name for user. Name is case sensitive.</p>
        <p></p>
        <p><code>User.setProperty(&quot;city&quot;, &quot;London&quot;, &quot;string&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.getProperty(name)</code>
      </td>
      <td style="text-align:left">
        <p>Read property with name. Name is case sensitive.</p>
        <p></p>
        <p><code>User.getProperty(&quot;city&quot;)</code>
          <br />
          <br />can get property with default value for non exist property:</p>
        <p><code>User.getProperty(&quot;city&quot;, &quot;London&quot;)</code>
        </p>
        <p>&lt;code&gt;&lt;/code&gt;</p>
        <p>can get property of another bot:</p>
        <p><code>  </code>Bot.getProperty({ name: &quot;propName&quot;, other_bot_id: <code>OTHER_BOT_ID })</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.addToGroup(group_name)</code>
      </td>
      <td style="text-align:left">
        <p>Add user to group with group_name</p>
        <p></p>
        <p><code>User.addToGroup(&quot;guests&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.getGroup()</code>
      </td>
      <td style="text-align:left">Get current user&apos;s group</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.removeGroup()</code>
      </td>
      <td style="text-align:left">Remove user from current group</td>
    </tr>
  </tbody>
</table>

**Access to property** in answer:

> You can also use the properties in the command's answer. For example, you can do this with the / hello command:`Hello, <UserRole>!`

in BJS:

> And you can use it in `Bot.sendMessage("Hello, <UserRole>")`

