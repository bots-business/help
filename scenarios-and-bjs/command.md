# Command

| Function | Description | Example |
| :--- | :--- | :--- |
| `Command.setValue(name, value)` | set value to command | `Command.setValue("url", "http://example.com")` |



**You can pass a variable to the URL of the previous script.** This is useful if you need to generate a URL.

```text
->(<my_url>)
```

The variable `my_url` needs to be set in the previous script of this command:

```javascript
Command.setValue("my_url", "http://example.com");

->(<my_url>)
```

Here in the first scenario, the variable `my_url` is set. In the second scenario, the page is loaded from [http://example.com](http://example.com/).

> You can set `my_url` in any other command with function `Command.setValue`

