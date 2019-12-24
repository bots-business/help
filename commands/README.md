---
description: What it is "bot command"?
---

# Commands

Command - it is text from user. Bot can sent answer for command or do something. Usually command start with `"/"`, e.g. `/hello`. But it is not always required.

{% hint style="warning" %}
`/start` and `/START` - it is not same commands. Command is case sensitive
{% endhint %}

### How to execute command with any text from user? \(Master command\)

Just use `*` in command name. 

See [more](https://help.bots.business/scenarios-and-bjs/always-running-commands)



### Command's fields

Command can have:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>command</code>
      </td>
      <td style="text-align:left">use for command call</td>
      <td style="text-align:left">
        <p><code>&quot;/start&quot;</code>, <code>&quot;/run&quot;</code>. For any
          text use <code>&quot;*&quot;</code>
        </p>
        <p><b>case sensitive</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>help</code>
      </td>
      <td style="text-align:left">command&apos;s description</td>
      <td style="text-align:left"><code>&quot;Welcome to RentBot. See /help or /order now&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>answer</code>
      </td>
      <td style="text-align:left">text answer</td>
      <td style="text-align:left"><code>&quot;Hello. You need /register before continue&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>aliases</code>
      </td>
      <td style="text-align:left">use for alternative command call</td>
      <td style="text-align:left"><code>&quot;/welcome, /hello, ?, help&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>keyboard</code>
      </td>
      <td style="text-align:left">send keyboard to user on command call</td>
      <td style="text-align:left"><code>&quot;order, about&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>scenarios</code>
      </td>
      <td style="text-align:left"><code>BJS</code> code for execution</td>
      <td style="text-align:left"><code>2+2</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>group</code>
      </td>
      <td style="text-align:left">command allowed only for this user&apos;s group</td>
      <td style="text-align:left"><code>guests</code>, <code>clients</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>need_reply</code>
      </td>
      <td style="text-align:left">command wait for answer from user. Can be true, false or blank. If <code>true</code> BJS
        code execute after users&apos;answer</td>
      <td style="text-align:left"><code>true</code>, <code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>auto_retry</code>
      </td>
      <td style="text-align:left">command can be runs with interval in secs</td>
      <td style="text-align:left"><code>600</code> - repeated once at 10 minutes</td>
    </tr>
  </tbody>
</table>### How to create and edit commands?

**You can edit command directly from application.**

![Screen from App for command creation](../.gitbook/assets/image%20%2814%29.png)

### Commands importing

Make all commands with [Google Table. ](https://help.bots.business/create-bot-from-google-table)

Also you can copy Template table from [http://bit.ly/bb\_table\_template](http://bit.ly/bb_table_template) into your own table. 

This is an ideal option: everything is quite simple. Go to `Main menu > File > Make a copy`. You will need a separate sheet for the commands that will contain the commands. Then you can add commands in rows and do the CSV import from application.





### 



