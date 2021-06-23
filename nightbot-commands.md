# Nightbot Commands

## Basic building blocks

Remember that a command doesn't have to start with ! - many of the most fun commands are triggered on normal words, like "hello" or "FRICK"

Counter:

```js
This is a command which counts from 1 to $(count)
```

Random selection from a list:

```js
$(eval things=["list", "of", "things"]; things[Math.floor(Math.random() * things.length)])
```

Percentage chance:

```js
There is a $(eval Math.ceil(Math.random()*100))% chance Caite will kill another jellyfish in the next 5 minutes
```

Use decoded query input or the username (hat tip sleepy__dan__)

```js
$(eval input=decodeURIComponent("$(querystring)" || "$(user)"); input)
```

Chance of returning something or nothing - this tests the random() thing for an arbitrary barrier, and returns an empty string if it's not passed, which is just ignored by the twitch message API.

```js
$(eval Math.random() < 0.1 ? "@$(user) " + decodeURIComponent("$(querystring)") : " ")
```


## Useful

### !pickagame

Pick a random line from a ; seperated list of lines on pastebin

```js
@caitelatte you should play $(eval notes = "$(urlfetch json https://pastebin.com/raw/2WWqUWTv) ".split(";"); notes[Math.floor(Math.random() * (notes.length - 1))])
```

### !time

```js
streamer's local time is $(time Australia/Sydney "h:mm a z").
```

## Fun

### !beans

A chatter check for 1-100 beans of various types. Can be easily changed for other checks!

```js
@$(touser) you get $(eval Math.floor(Math.random() * 100)) $(eval beans=["coffee beans LoveBean", "lima beans", "red beans", "red beans from the video game celeste", "cute beans", "cat beans", "black beans", "beanbag beans", "moon beans from the video game clestee MoonBean", "updog beans"]; beans[Math.floor(Math.random() * beans.length)] )
```

### celeste/shuffle

scramble the letters in a set string, or very rarely return "cheese"

```js
$(eval Math.random() > 0.0001 ? "celeste".split('').sort(function(a, b) {return 0.5 - Math.random();}).join("") : "cheese")
```

### Nightbot dad jokes

"no u" command: `nightbot` -> `$(user) $(query)`

> caitelatte: nightbot is stinky
> nightbot: caitelatte is stinky

"hi hungry i'm dad command" `i'm`, with an alias on `im`

```js
$(eval Math.random() < 0.1 ? "hi " + decodeURIComponent("$(querystring)") + " i'm nightbot" : " ")
```

## Chaotic

### !angy

Replace sets of owo/uwu/o-o/e_e with angry accented versions of the eyes

```js
$(eval accentdict={"o": "ò$1ó", "u": "ù$1ú", "e": "è$1é", "O": "Ò$1Ó", "U": "Ù$1Ú", "E": "È$1É"}; function angy(match, p1, p2, p3, p4) {return accentdict[p2].replace("$1", p3)}; decodeURIComponent("$(querystring)").replace(/(([eou])([wn\-_])(\2))/ig, angy) )
```

### !celeste

Scramble spelling of "celeste" with an extra e every time it's run

```js
$(eval ("e".repeat(Math.min($(count), 393))+"celeste").split('').sort( function(a, b) { return 0.5 - Math.random(); }).join("") )
```

### !m

replace all n's with m, and add an extra m to certain parts of each word.

```js
$(eval imput=decodeURIComponent("$(querystring)"||"$(user)").split(" "); for (var i = 0; i < imput.length; i++) { imput[i] = imput[i].replace(/n/gi, 'm'); mamtches = imput[i].match(/([aeiou])([^aeioum])/ig); if (mamtches) {mamtch = mamtches[Math.floor(Math.random()*mamtches.length)]; imput[i] = imput[i].replace(mamtch, mamtch.replace(/(.)(.)/, "$1m$2"))} }; imput.join(" ") )
```

### !sple

funny command to randomly shift the main letters on a US keyboard one to the left

```js
$(eval var allkeys = "`1234567890-=qwertyuiop[]\\asdfghjkl;'zxcvbnm,./"; var allkeys_shifted = "1234567890-=`wertyuiop[]\\qsdfghjkl;'axcvbnm,./z"; function shiftkey(inkey) {return Math.random() < 0.2? allkeys_shifted[allkeys.indexOf(inkey)] || inkey : inkey}; (decodeURIComponent("$(querystring)")||"$(user)").toLowerCase().replace(/./g, shiftkey) )
```

### !wide

add a space between each letter of the command (using a regex instead of join for absolutely no good reason)

```js
$(eval decodeURIComponent("$(querystring)").replace(/./g, '$& ') )
```