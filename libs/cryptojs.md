# CryptoJS

JavaScript library of crypto standards.

## Usage

```javascript
var hash = CryptoJS.HmacSHA1("Message", "Key")
Bot.sendMessage(String(hash));

hash = CryptoJS.HmacSHA256("Message", "Secret Passphrase");
Bot.sendMessage(String(hash));
```

Documentation: [https://cryptojs.gitbook.io/docs](https://cryptojs.gitbook.io/docs/)

