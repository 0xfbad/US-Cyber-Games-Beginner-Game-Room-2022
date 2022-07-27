# Miscellaneous
[Tick Tac Toe](#tick-tac-toe---2pts)<br>
[No Punctuation Here](#no-punctuation-here---3pts)<br>
[Circle of Fraud](#circle-of-fraud---6pts)<br>
[Sekrit](#sekrit---10pts)<br>
[Sunlight and SQL](#sunlight-and-sql---12pts) *no soln*<br>
[This is puzzling?!?](#this-is-puzzling---15pts)<br>
[Hidden Treasure](#hidden-treasure---20pts) *no soln*<br>

## Tick Tac Toe - 2pts
> What linux command could you use to reorder a log file contents from last to first?
> 
> For example,
> 
> from:
> 
> line1
> line2
> line3
> line4
> 
> to:
> 
> line4
> line3
> line2
> line1
> 
> Author: WSC

Opposite of `cat` is the `tac` command.

Answer:
```
tac
```

## No Punctuation Here - 3pts
> What command could you use with "tr" on a string of characters to only show letters and numbers?
> 
> Author: WSC

Instead of only printing letters/numbers, just delete (`-d`) special characters (`[:punct:]`).

Answer:
```
-d [:punct:]
```

## Circle of Fraud - 6pts
> A riddle*, wrapped in a mystery**, inside an enigma***.
> 
> \* flag<br>
> ** program<br>
> *** encoding<br>
> Created by US Cyber Range
> 
> [enigma.txt]()

Looking into `enigma.txt`, we see this text:
```
KD08X3EjITd9NVh6aURDLy5AdGIqPCgtbm03NjVpaEQxVFMuYixPdTt5eHc3JFdWRWtTby9RbD5NdnV0YGVHY0VhM358MFw+VFk7OjkoOHFMcCNuSEdZLnt6LCoqRWN0TzteXm9bSWxrM3o3Nnd2dVBzMHBNS10rbGsoJ2clJHtBIXh9fHU7XHhxdm9uNHJxcGlubWxramMpS2ZlZGNiW2BZfUBcW1R4WFdWVVQ2UktvTzFNRkVEaEJBQEU+YkJBQD8+PTx8OjkyVjYvLjMyMTAvKEwsJSQpKCcmfUN7eiF4fXx7dDpbcXB1dHNyazFSaGdsa2QqS2dmZV4kXGFgWV5dVlV5U1JRUHRUTVJLb09OTUwuSklCZkZFJ0NCQUA/PjdbWnszMjc2NS5SMiswKU0nLCsqKSgnfkRlI3pAfn12dTtzcnFwbzVWcmsxaW5tZixNYylnZkhkXWJhYFlYfHtbVFlYV1ZVVE1xNFBJSEdrLkRDSEdAZD49PDtfPzg9NjU6OTgxVXY0LVExMC8uLSwrKikoJyYlJGR6eXh3fHt0Oic=
```

This looks like Base 64 encoding (because of the `=` padding) so decoding it we get:
```
(=<_q#!7}5XziDC/.@tb*<(-nm765ihD1TS.b,Ou;yxw7$WVEkSo/Ql>Mvut`eGcEa3~|0\>TY;:9(8qLp#nHGY.{z,**EctO;^^o[Ilk3z76wvuPs0pMK]+lk('g%${A!x}|u;\xqvon4rqpinmlkjc)Kfedcb[`Y}@\[TxXWVUT6RKoO1MFEDhBA@E>bBA@?>=<|:92V6/.3210/(L,%$)('&}C{z!x}|{t:[qputsrk1Rhglkd*Kgfe^$\a`Y^]VUySRQPtTMRKoONML.JIBfFE'CBA@?>7[Z{32765.R2+0)M',+*)('~De#z@~}vu;srqpo5Vrk1inmf,Mc)gfHd]ba`YX|{[TYXWVUTMq4PIHGk.DCHG@d>=<;_?8=65:981Uv4-Q10/.-,+*)('&%$dzyxw|{t:'
```

This looks like another layer of encoding, specifically, it looks like Base 91 encoding. However, decoding from Base 91, we get binary data. So I assume its some kind of esoteric language. Looking online I found something called malbolge, it looks pretty similar:

![malbolge](https://i.imgur.com/klnSGqk.png)

Finding an [online interpreter for malbolge](https://www.malbolge.doleczek.pl/), we can see the output of the following code:

![malbolge](https://i.imgur.com/RvtngUA.png)

And we got the flag!, answer:
```
flag{its_r3d_and_sm3lls_lik3_bloo_paint}
```

## Sekrit - 10pts
> Secret Squirrel (@sekritskwerl) likes to communicate with field operatives using hidden messages in his social media feeds. Can you discover his latest message? You'll have to do some digging to get all of the info you need to discover the flag!
> 
> Created by US Cyber Range

To start, googling gives us a few things, one being a site. However, because the challenge is asking about social media, we will ignore it.

Finding the twitter user, we can see that they've uploaded some pictures and retweeted a few tweets.

![Twitter User](https://i.imgur.com/A849Vj7.png)

A hint:

![Hint](https://i.imgur.com/I7RogcR.png)

Another hint:

![Hint](https://i.imgur.com/JsBln0t.png)

Downloading the image, we can run `steghide` and try to extract something with a password. We'll try `flatearth` as the password.
```
┌──(kali㉿kali)-[~]
└─$ steghide extract -sf DzQF5B6XcAA1LFN.jpg
Enter passphrase: flatearth
wrote extracted data to "flag.txt".
```

Print out the text file and we get the flag:
```
┌──(kali㉿kali)-[~]
└─$ cat flag.txt
BeSureToDrinkYourOvaltine
```

And we got the flag:
```
BeSureToDrinkYourOvaltine
```

## Sunlight and SQL - 12pts
> Internal note: you'll probably get reports that this host is down even when it's not. ICMP is disabled, so ping and default nmap will tell you that it's down. There's nothing on port 80, so opening it in a browser also won't do anything.
> 
> We've been trying to inventory devices on our network, but our team keeps stumbling upon servers that we have no clue about what they do or how to connect to them. Can you help us see what's stored at `impostor.uscyber.mctf.io`?
> 
> Created by MetaCTF

```
NO SOLUTION YET
```

## This is puzzling?!? - 15pts
> Happy (late) Valentine's Day to the Old Dominion!<br>
> Not easy, but straightforward. It might take a few tries to get this right, but there is a limit!
> 
> Created by US Cyber Range
> 
> [PUZZLR.7z]()

Given a gcode (instructions for a 3d printer/cnc on where to move) file, we're able to insert it into an online viewer and get the following:

![PUZZLR](https://i.imgur.com/2hK9SIx.jpg)

After combining the pieces you get the following output:

![PUZZLR](https://i.imgur.com/cQmIfhI.png)

Answer:
```
viRgiNiaIZ4LuVrz
```

## Hidden Treasure - 20pts
> One of our targets has a way he uses to obtain some sensitive information. We want to get that information. His name is Durukan Topaloglu. We know that he has done some development work before.
> 
> Created by MetaCTF

Alright first things off, lets google the name of the target.

![Google search](https://i.imgur.com/Kn74RkD.png)

Looks very empty, lets look at Shodan for more information.

![Site](https://i.imgur.com/YQFnz4K.png)

We found a secret domain, hmm:

![Shodan](https://i.imgur.com/VpKgwd6.png)

Lets visit the site:

![Site](https://i.imgur.com/Z1os5gW.png)

```
NO SOLUTION YET
```