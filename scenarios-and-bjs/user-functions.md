# Функции пользователей

<table>
  <thead>
    <tr>
      <th style="text-align:left">Функция</th>
      <th style="text-align:left">Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>User.setProperty(name, value, type)</code>
      </td>
      <td style="text-align:left">
        <p>Установить свойство с названием для пользователя</p>
        <p></p>
        <p><code>User.setProperty(&quot;city&quot;, &quot;London&quot;, &quot;string&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.getProperty(name)</code>
      </td>
      <td style="text-align:left">
        <p>Прочесть свойство, с названием, пользователя</p>
        <p></p>
        <p><code>User.getProperty(&quot;city&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>User.addToGroup(group_name)</code>
      </td>
      <td style="text-align:left">
        <p>Добавить пользователя в определенную группу с названием_группы</p>
        <p></p>
        <p><code>User.addToGroup(&quot;guests&quot;)</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>**Доступ к свойствам** в ответе:

> Вы также можете использовать свойства в ответе команды. Например, вы можете сделать это с помощью команды / hello: `Hello, <UserRole>!`

в BJS:

> И вы можете использовать его в виде `Bot.sendMessage("Hello, <UserRole>")`

