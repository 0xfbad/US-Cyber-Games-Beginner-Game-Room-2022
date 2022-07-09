# Cryptography

## Number Based Decoding - 8l4ckh4t - 1pts
> 63 68 61 72 67 65 20 2E 30 33 20 62 69 74 63 6F 69 6E 2066 6F 72 20 64 65 63 72 79 70 74 20 6B 65 79 73
> 
> Created by Nick F.

Looks like hex, lets chuck it into CyberChef:

![Site](https://i.imgur.com/e6QMoNE.png)

Answer:
```
charge .03 bitcoin for decrypt keys
```

## n1nj4 - 2pts
> This message appears to be a password and seems to be obfuscated using a popular cryptographic algorithm:
> NDgyYzgxMWRhNWQ1YjRiYzZkNDk3ZmZhOTg0OTFlMzg=
> 
> Created by Nick F.

The `=` hints there's base64 padding, meaning that this is a base64 encoded string.

Lets chuck it into CyberChef again:

![Site](https://i.imgur.com/sXMf6n8.png)

Though now we get `482c811da5d5b4bc6d497ffa98491e38` which doesn't seem like a flag. Instead it looks like a hash.

```
┌──(kali㉿kali)-[~]
└─$ hash-identifier 482c811da5d5b4bc6d497ffa98491e38
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

Least Possible Hashs:
...
```

I was right, its a `MD5` hash. But since MD5 is a one-way hash, it would take a while to brute force it. But checking online I found a database checker has stored common MD5 hashes and their values.

![MD5](https://i.imgur.com/TP0JFDQ.png)

And we have our flag:
```
password123
```

## Key - r00t - 3pts
> Our analysts at the SOC command center have discovered a few files and believe they are related to one another. First, figure out what the first set of numbers mean. This file was labeled “KEY.”
> 
> 75 6E 6272 65 61 6B 61 62 6C 65 52 53 41
>
> Created by Nick F.

Looks like Hex, lets chuck it into CyberChef:

![Site](https://i.imgur.com/M0e7dgg.png)

Answer:
```
unbreakableRSA
```

## Bad Event - USA - 4pts
> Our analysts at the SOC command center have noticed that the same block of emojis has been showing up in hacker communications. Analysts believe they are using emojis to represent important information.
> 
> Created by Nick F.
> 
> [bad-event-usa]()

Given this image, we need to obtain a flag:

![bad-event-usa](https://i.imgur.com/MZhTRqn.png)

The emojis used are:
```
🦵 - 208 - U+1F9B5
😰 - 87 - U+1F630
🤍 - 149 - U+1F90D
👵 - 250 - U+1F475
```

Hmm, four periods and some emoji codes? Sounds like an ip address...

Submission:
```
208.87.149.250
```

## Going Home - 4pts
> Our analysts at the SOC command center have noticed that the same block of emojis has been showing up in hacker communications. Analysts believe they are using emojis to represent important information.
> 
> Created by Nick F.
> 
> [going-home-file]()

Another emoji file:

![going-home-file](https://i.imgur.com/yZ8Xk3O.png)

Emojis are:
```
💋 - 127 - U+1F48B
😀 - 1 - U+1F600
```

Converting into an ip address:
```
127.0.0.1
```

## Message - m4st3r - 5pts
> After deciphering the “KEY,” move on and use this information to help solve the second related message. The message allegedly contains a high-value target address and the sooner we figure it out the better!
> 
> bnVmIGtlcnFldSB0dyBjZ2N1Z2Z1IGV0IDk4MDAgY2F3bGt2IGpk
> 
> Created by Nick F.

Throwing this into CyberChef, I can see a high entropy from decoding with Base64:

![Site](https://i.imgur.com/W4xkbk8.png)

Looks like a Vigenere cipher, lets try it with the key we got previously in `Key - r00t`:

![Site](https://i.imgur.com/D9nXV3j.png)

And we got our flag:
```
the target is located at 9800 savage rd
```

## Decoding Flags - m4dh4t - 6pts
> Our analysts at the SOC command center have found some pictures that are hiding a flag. They need help decoding the message.
> Created by Nick F.
> 
> [Decoding flags]()

So given this image, we need to obtain a flag:
![Decoding flags](https://i.imgur.com/ZDKspno.png)

These look like nautical signalling flags, so lets find an online dictionary to help us:

![Online dictionary](https://i.imgur.com/h3yvkLS.png)

Comparing the two I got the flag, Answer:
```
flag_cyber_5946
```

## Bad Event - China - 8pts
> Our analysts at the SOC command center have noticed that the same block of emojis has been showing up in hacker communications. Analysts believe they are using emojis to represent important information.
> 
> Created by Nick F.
> 
> [bad-event-china]()

Another emoji set we need to decode:

![bad-event-china](https://i.imgur.com/zuy8k5A.png)

The emojis used are:
```
✍ - 202 - U+270D
👻 - 111 - U+1F47B
🤏 - 175 - U+1F90F
🤭 - 31 - U+1F92D
```

Just like last time, four periods seems like an ip address. Decoding this gives us:
```
202.111.175.31
```