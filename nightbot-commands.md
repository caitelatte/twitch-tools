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

### !blat

let chat make a bacon-loaded [bacon lettuce avocado tomato sandwich](https://www.taste.com.au/recipes/blat/5730389b-354a-4cca-a7a1-ca4049dfd201)! code based on the !celeleste shuffling command.

```js
# all one line for nightbot command
function l() {bWeight = 5; lWeight = 2; endWeight = 1; total = bWeight + lWeight + endWeight;roll = Math.random()*total; return ( roll <= bWeight ? '🥓' : roll <= bWeight + lWeight ? '🥬': '' ) }; var outtext = Array(); next = { "🍞": "🥓", "🥑": "🍅", "🍅": "🍞" }; outtext = ["🍞"]; for (var i = 1; i < 100; i ++) { nextletter = (next[outtext[i-1]] || l()); if (!nextletter) { outtext.push("🥑","🍅", "🍞"); break; } else { outtext.push(nextletter) } }; outtext.join("")

# expanded so you can actually read it
function l() {
  bWeight = 5;
  lWeight = 2;
  endWeight = 1;
  total = bWeight + lWeight + endWeight;
  roll = Math.random()*total;
  return ( roll <= bWeight ? '🥓' : roll <= bWeight + lWeight ? '🥬': '' )
};
var outtext = Array();
next = { "🍞": "🥓", "🥑": "🍅", "🍅": "🍞" };
outtext = ["🍞"];
for (var i = 1; i < 100; i ++) {
  nextletter = (next[outtext[i-1]] || l());
  if (!nextletter) {
    outtext.push("🥑","🍅", "🍞");
    break;
  } else {
    outtext.push(nextletter)
  }
};
outtext.join("")
```

## Chaotic

### !angy

Replace sets of owo/uwu/o-o/e_e with angry accented versions of the eyes, and replaces `r` and `l` with w for the funny uwu text. Set up !angwy as an alias to !angy so it does a combination of both commands!

```js
!addcom !uwu -cd=5 $(eval var input_text=decodeURIComponent(`$(querystring)` || '$(user)'); input_text.replace(/[lr]/gi, "w").replace(/([uo])(\1)/gi, "$1w$2");)

!addcom !angy -cd=5 $(eval accentdict={"o": "ò$1ó", "u": "ù$1ú", "e": "è$1é", "O": "Ò$1Ó", "U": "Ù$1Ú", "E": "È$1É"}; function angy(match, p1, p2, p3, p4) {return accentdict[p2].replace("$1", p3)}; decodeURIComponent("$(querystring)").replace(/(([eou])([wn\-_])(\2))/ig, angy) )

!addcom !angwy -cd=5 -a=!angy $(eval var input_text=decodeURIComponent(`$(querystring)` || '$(user)'); input_text.replace(/[lr]/gi, "w").replace(/([uo])(\1)/gi, "$1w$2");) owo
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

### !time

Generate a fake time within your general time range with the current correct minute.

```js
it's probably like $(eval hour=(Math.floor(Math.random()*10) + 18) % 24; datehastobeanobject = new Date(); hour.toString().padStart(2, "0") + ":" + datehastobeanobject.getMinutes().toString().padStart(2, "0") )
```

### !wide

add a space between each letter of the command (using a regex instead of join for absolutely no good reason)

```js
$(eval decodeURIComponent("$(querystring)").replace(/./g, '$& ') )
```
