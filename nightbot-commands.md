# Nightbot Commands

## Basic building blocks

Remember that a command doesn't have to start with ! - many of the most fun commands are triggered on normal words, like "hello" or "FRICK"

*   Counter:

    ```js
    This is a command which counts from 1 to $(count)
    ```
    
    *   Reset the count of a counter: make the !setcount command an alias to !commands, and make sure you enter the code for this one in your dashboard - if you enter it in chat, I find it sets the counter of !setcount and not the target one.

        ```js
        # if your mods just want an easy command to edit any counter, use this like
        # !setcount !testingcount 10
        edit $(1) -c=$(2)
        
        # if you just want to sometimes reset a specific counter to 0
        edit !testingcount -c=0
        ```

*   Random selection from a list:

    ```js
    $(eval things=["list", "of", "things"]; things[Math.floor(Math.random() * things.length)])
    ```

*   Percentage chance:

    ```js
    There is a $(eval Math.ceil(Math.random()*100))% chance Caite will kill another jellyfish in the next 5 minutes
    ```

*   Use decoded query input or the username (hat tip sleepy__dan__)

    ```js
    $(eval input=decodeURIComponent("$(querystring)" || "$(user)"); input)
    ```

*   Chance of returning something or nothing - this tests the random() thing for an arbitrary barrier, and returns an empty string if it's not passed, which is just ignored by the twitch message API.

    ```js
    $(eval Math.random() < 0.1 ? "@$(user) " + decodeURIComponent("$(querystring)") : " ")
    ```

*   Up to 5 random elements picked from a list:

    ```js
    $(eval extras=["cope","mald","Burgy","YEEHAWKING","touch grass","KirbyBlob","banned","miau"];output=[];for (i = 0;i<Math.ceil(Math.random()*5);i++) {output.push(extras[Math.floor(Math.random()*extras.length)])};output.join(" + ") )
     ```

## Useful

### !shoutout/!so

Shout out a fellow streamer! Maybe they've just raided in or maybe you just think they're very cool.

```js
$(twitch $(touser) "Go check out {{name}} over at {{url}} They were last playing {{game}}!")
```

### !pickagame

Pick a random line from a ; seperated list of lines on pastebin

```js
@caitelatte you should play $(eval notes = "$(urlfetch json https://pastebin.com/raw/2WWqUWTv) ".split(";"); notes[Math.floor(Math.random() * (notes.length - 1))])
```

### !time

```js
streamer's local time is $(time Australia/Sydney "h:mm a z").
```

### !wiki
Get a quick Wikipedia summary of a topic being conversed in chat.
```js
$(eval a=$(urlfetch json https://en.wikipedia.org/w/api.php?action=query&prop=extracts&exchars=380&exintro=true&titles=$(querystring)&explaintext=1&formatversion=2&format=json&origin=*)['query']['pages'][0]['extract'];)
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

"no u" command: `!addcom nightbot $(user) $(query)`

> caitelatte: nightbot is stinky
> nightbot: caitelatte is stinky

"hi hungry i'm dad" commands

```js
$(eval Math.random() < 0.1 ? "hi " + decodeURIComponent("$(querystring)") + " i'm nightbot" : " ")
$(eval query=decodeURIComponent("$(querystring)").match(/^am(.*)/i); (query != null && Math.random() < 0.1) ? "hello " + query[1] : " " )
```

To add the full suite of funny dadjokes:

```js
!addcom -cd=5 i'm $(eval Math.random() < 0.1 ? "hi " + decodeURIComponent("$(querystring)") + " i'm nightbot" : " ")
!addcom -cd=5 -a=i'm im
!addcom -cd=5 i $(eval query=decodeURIComponent("$(querystring)").match(/^am(.*)/i); (query != null && Math.random() < 0.1) ? "hello " + query[1] : " " )
!addcom nightbot $(user) $(query)
```

## Food

### !blat / !blt / !vegesandwich / !queermeal

Let chat make a bacon-loaded [bacon lettuce avocado tomato sandwich](https://www.taste.com.au/recipes/blat/5730389b-354a-4cca-a7a1-ca4049dfd201)! Alternatively, a !BLT (no avo), a !vegesandwich, or a !queermeal with a food item from each letter in LGBTQIA! You can also build on !friedrice to make your own menu item that involves any base item (the rice emoji) and a selection of up to ten other ingredients :)

```js
# !blat
$(eval function l() {bWeight = 5; lWeight = 2; endWeight = 1; total = bWeight + lWeight + endWeight;roll = Math.random()*total; return ( roll <= bWeight ? '🥓' : roll <= bWeight + lWeight ? '🥬': '' ) }; var outtext = Array(); next = { "🍞": "🥓" }; outtext = ["🍞"]; for (var i = 1; i < 100; i ++) { nextletter = (next[outtext[i-1]] || l()); if (!nextletter) { outtext.push("🥑","🍅", "🍞"); break; } else { outtext.push(nextletter) } }; outtext.join("") )

# !blt
$(eval function l() {bWeight = 5; lWeight = 2; endWeight = 1; total = bWeight + lWeight + endWeight;roll = Math.random()*total; return ( roll <= bWeight ? '🥓' : roll <= bWeight + lWeight ? '🥬': '' ) }; var outtext = Array(); next = { "🍞": "🥓" }; outtext = ["🍞"]; for (var i = 1; i < 100; i ++) { nextletter = (next[outtext[i-1]] || l()); if (!nextletter) { outtext.push("🍅", "🍞"); break; } else { outtext.push(nextletter) } }; outtext.join("") )

# !vegesandwich
$(eval vegetals = "🫒 🥑 🍆 🥕 🌽 🌶️ 🫑 🥒 🥬 🍄 🥚 🧂 🧀 🍅 🍳".split(" "); height = Math.floor(Math.random()*10); sandwich = ["🍞"]; for (i=0;i<height;i++) {sandwich.push(vegetals[Math.floor(Math.random()*vegetals.length)])}; sandwich.push("🍞"); sandwich.length > 2 ? sandwich.join("") : "🍞🧈🍞" )

# !queermeal
@$(user) here's your LGBTQIA+ meal of the day: $(eval L=["🍋","🥬","🍗","🍭","🍬","☕","🥛"];G=["🍇","🧄","🥗","🥟","🧃","🍷"];B=["🍓","🍌","🫐","🫑","🥦","🍞","🥖","🥯","🥓","🥩","🍔","🌯","🥫","🍱","🧈","🧋","🍺"];T=["🍅","🌮","🍤","🥡","🫖","☕"];Q=["🧀"];I=["🍨","🧊","🍦","🍛"];A=["🍎","🍏","🍍"];function rand(inlist){return inlist[Math.floor(Math.random()*inlist.length)]};[rand(L),rand(G),rand(B),rand(T),rand(Q),rand(I),rand(A)].join("") )

# !friedrice
$(eval ingredients = "🍆 🥕 🌽 🌶️ 🫑 🥬 🍄 🥚 🧂 🧀 🍳".split(" "); height = Math.ceil(Math.random()*10); frypan = ["🍚"]; for (i=0;i<height;i++) {frypan.push(ingredients [Math.floor(Math.random()*ingredients.length)])}; frypan.join("") ) 
```

<details>
  <summary>!blat code on separate lines so you can actually read it!</summary>
  ```js
  # !blat expanded so you can actually read it
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
</details>

### !fight
  
Fight a user or concept in a random game of random chance! Replace "CatChamp" and "Funk!" with the emotes of your choosing :)
  
```js
	$(user) fights $(query) in $(eval fights=["a game of wits", "a sea of thieves arena", "a throweline race", "code golf", "a game of chance", "a game of favour with the RNG god", "10x1a", "10x7b"]; fights[Math.floor(Math.random()*fights.length)]) and $(eval Math.random() > 0.5 ? "wins CatChamp" : "loses Funk!" )
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

replace all n's with m, and add an extra m to certain parts of each word for comemdic effemct.

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
  
### !ab and !ba
  
replace all vowels and consonants with the 🅰️🆎🅾️🅱️ emojis as appropriate! fully reversible with !ba, though there may be some information loss.
  
```js
$(eval input=decodeURIComponent("$(querystring)" || "$(user)"); input.replace(/o/ig, "🅾️").replace(/[aeiuy][^aeiu🅾️y ]/ig, "🆎").replace(/[aeiuy]/ig, "🅰️").replace(/[^aeiuy🅰️🆎🅾️ ]/ig, "🅱️") )
  
$(eval function random(match, offset, string) {match == "🅰️" ? word="aeiuy" : word="qwrtpsdfghjklzxcvbnm"; return word.split("")[Math.floor(Math.random()*word.length)] }; input=decodeURIComponent("$(querystring)"||"$(user)"); input.replace(/🅾️/g, "o").replace(/🆎/g, "🅰️🅱️").replace(/🅰️|🅱️/g, random).replace(/AAAA/g, "aaaa") )
```
