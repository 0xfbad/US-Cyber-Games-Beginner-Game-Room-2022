# Forensics
[Out of Sequence](#out-of-sequence---2pts) *no soln*<br>
[No Playoffs this Year](#no-playoffs-this-year---4pts)<br>
[Radio Message!!!](#radio-message---5pts) *no soln*<br>
[May the Force be with You](#may-the-force-be-with-you---8pts)<br>
[Most Wanted](#most-wanted---10pts) *no soln*<br>
[No Collision here.](#no-collision-here---12pts)<br>
[Stay in the Radius](#stay-in-the-radius---18pts) *no soln*<br>

## Out of Sequence - 2pts
> Our team has captured lots of TCP packets, but they look like they are out of sequence. Can you help us find what those packets are?
> 
> Submit the flag in the following format: ucgCTF{word_word_word}<br>
> Created by Biju B.
> 
> [doscapture.pcap]()

```
NO SOLUTION YET
```

## No Playoffs this Year - 4pts
> A hacker broke into our network and created an account. We figured out what new account was created in Active Directory, but are unsure of the hashed password. Can you find the password that the hacker used?<br>
> Created by Jesse V.
> 
> [no-playoffs-this-year]()

First off, we set a search to scan all packet bytes (this is the packet byte section):

![Packet Bytes](https://i.imgur.com/eHoqCEm.png)

Search query:

![Search](https://i.imgur.com/TCECQO0.png)


We find a hit:

![Hit](https://i.imgur.com/rAY8Uvw.png)


And in the server TCP stream we can see the password:

![TCP Stream](https://i.imgur.com/ezdQV8p.png)

User input:
```
..%........'.......P............%.......'.....'..SFUTLNTVER.2.SFUTLNTMODE.Console..administrator
P@ssw0rd
.......ANSI..net start
net user
net user lebronjames FLAG-LAKERS2022 /add
y
exit
```

Answer:
```
FLAG-LAKERS2022
```

## Radio Message - 5pts
> One friend from overseas wants to visit me in my hometown in southeast Tennessee. I gave him my contact number. During his visit, he sent me a secret message using radio imaging. But someone intercepted the message.
> 
> Submit the flag in the following format: ucgCTF{word_word_word}<br>
> Created by Biju B.
> 
> [radiomessage.pcap]()

```
NO SOLUTION YET
```

## May the Force be with You - 8pts
> Directions: A hacker has compromised your machine. Analyze the PCAP and find what data was exfiltrated from the network. The force will be with you, always.<br>
> Created by Jesse V.
> 
> [may-they-force-be-with-you]()

Opening the provided pcap file, you can tell that there's FTP traffic, which is unencrypted and in plaintext.

![FTP traffic](https://i.imgur.com/pp6ZPGC.png)

Looking into said FTP traffic, we find a file called `plans.jpg` being transferred:

![FTP traffic](https://i.imgur.com/c0VEmDw.png)

Filtering `ftp-data`, we see the packets containing the file.

![FTP traffic](https://i.imgur.com/0zxP42C.png)

To extract, we can right click any of the packets and follow the TCP packet.

![FTP traffic](https://i.imgur.com/Su5JPq8.png)

Here we can see the data for the file.

![FTP traffic](https://i.imgur.com/QZUl4Dg.png)

Convert it into RAW and save as `plans.jpg`.

![FTP traffic](https://i.imgur.com/z0QGR3F.png)

Opening the file, we see the following:

![plans.jpg](https://i.imgur.com/QiaPs5t.jpg)

Answer:
```
FLAG-2022-HW
```

## Most Wanted - 10pts
> THE SCENARIO:<br>
> An unknown 2020 Austin arson/rioter is suspected of murder. Inspectors found a lethal pool of blood at the scene of the burned-out building, but the body is missing and there is no hard evidence except for a phone the lead investigator found the day after the building burning and murder. The investigator thinks it might be the perp’s phone (no usable fingerprints), so the investigator cloned the phone’s camera/SD card, signed the SD card images over to you, and is asking you, the lead digital forensics expert, to see what you can find.
> 
> The Inspector reports that the file system contains some photos, but a couple of photos from the time of the crime appear to be missing. If this is the perp’s phone and they deleted any photos, the investigator wants you to recover them and see if there’s any pertinent evidence that could help solve the case and locate the perp.
> 
> You were signed over an original and “WORKING” copy of the phone’s DCIM SD card, cloned from the suspect phone:
> 
> $ ls -la<br>
> -rw-rw-r-- 1 tweeks tweeks 4194304 Jul 19 19:56 phone-filesystem.img<br>
> -rw-rw-r-- 1 tweeks tweeks 4194304 Jul 19 19:56 phone-filesystem.img_WORKING
> 
> The chain of custody for this logical evidence has been tagged, logged, and signed over to you. Take care to only work on the .img_WORKING image. Take detailed, time-stamped notes and turn all findings in to the Inspector, and dd-erase all logical evidence once done.
> 
> The Inspector wants as much info as possible about the owner of this phone, but the ideal answer would be the owner’s full name.
> 
> FLAG: Use your digital forensic and OSINT skills to get the full name of the rioter/murder suspect.
> 
> Created by US Cyber Range
> 
> [phone-filesystem.tgz]()

```
NO SOLUTION YET
```

## No Collision here - 12pts
> There is an image of a Windows system provided to you. Find the file on an extension with this MD5 hash: 4a729045229e0c1dc0a471c589b3ee7d.<br>
> Created by Jesse V.
> 
> [disk-image]()

Given a massive directory of files, we need to check for a file with a MD5 hash. First we need to extract all files from the ntfs images. Once everything is extracted, we can run a simple command line as followed (this may take a bit since MD5 is a bit slow and there is a lot of files):
```
┌──(kali㉿kali)-[~]
└─$ find -type f -exec md5sum {} + | grep 4a729045229e0c1dc0a471c589b3ee7d
4a729045229e0c1dc0a471c589b3ee7d  ~/inetpub/logs/LogFiles/FTPSVC2/u_ex131112.log
```

To explain the command:
- `find` - the find command used to search for files in a directory hierarchy
- `-type f` - option to specify all files (non directories/special files/etc)
- `-exec md5sum {} +` - run `md5sum` on each file
- `| grep ...` - pipe the output and check if it contains the matching hash

Answer:
```
u_ex131112.log
```

## Stay in the Radius - 18pts
> John was told not to go outside the radius. John with his colleague pass to the radius. While staying within the limit of the radius, he captured data traffic from the LAN. But his colleague captured the wireless traffic. Both tried to analyze them but couldn’t find anything. They gave us their packet capture files. Can you help them find what was the secret?
> 
> Submit the flag in the following format: ucgCTF{word_word_word}<br>
> Created by Biju B.
> 
> [wirelesstraffic.pcap]()<br>
> [wiredtraffic.pcap]()

```
NO SOLUTION YET
```