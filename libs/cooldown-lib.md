# Cooldown Lib

Use this Lib to make cooldown.&#x20;

Cooldown have name and can be for user or for chat.

### Example

Command `/bonus`

```javascript
function onEnding(time){
  // can give bonus now
  Bot.sendMessage("You have bonus now");
  // your other code here
  //..

  return true; // if false - cooldown is not restarted
}

function onStarting(){
  // cooldown just started
  Bot.sendMessage("You will have bonus later");
}

function onWaiting(waitTime){
  // we have active cooldown
  Bot.sendMessage("Please wait: " + waitTime + " secs" );
}

Libs.CooldownLib.user.watch({
  // you need name for cooldown
  name: "GemBonusCooldown",
  time: 120, // cooldown time, 120 secs - 2 minute
  onStarting: onStarting,
  onEnding: onEnding,
  onWaiting: onWaiting
})
```



**or cool down for chat:**

```javascript
// or cooldown for chat:
Libs.CooldownLib.chat.watch({
  // you need name for cooldown
  name: "GemBonusCooldown",
  time: 120, // cooldown time, 120 secs - 2 minute
  onStarting: onStarting,
  onEnding: onEnding,
  onWaiting: onWaiting
})


// or global bot cooldown
// it can be used with Auto Retry (then chat or user are null)
/*
Libs.CooldownLib.watch({
  // you need name for cooldown
  name: "GemBonusCooldown",
  time: 120, // cooldown time, 120 secs - 2 minute
  onStarting: onStarting,
  onEnding: onEnding,
  onWaiting: onWaiting
})
*/
```



**get cool down:**

```javascript
// get current cooldown res for chat
let cooldown = Libs.CooldownLib.chat.getCooldown("GemBonusCooldown");

// for user:
// let cooldown = Libs.CooldownLib.user.getCooldown("GemBonusCooldown");

// global
// let cooldown = Libs.CooldownLib.getCooldown("GemBonusCooldown");

cooldown.value(); // current cooldown in second
cooldown.set(60 + cooldown.value()) // add 60 sec to cooldown
```







Command `/bust`

```javascript
// get current cooldown Res - see ResourcesLib
let cooldown = Libs.CooldownLib.chat.getCooldown("GemBonusCooldown");

// for user:
// let cooldown = Libs.CooldownLib.user.getCooldown("GemBonusCooldown");

// global
// let cooldown = Libs.CooldownLib.getCooldown("GemBonusCooldown");

var curValue = cooldown.value(); // current cooldown in second
cooldown.set(curValue - 40) // reduce 40 sec from cooldown
```

