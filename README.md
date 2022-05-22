Obsuc
--------
Obsuc is just a script that obscures urls. It's nothing fancy, just some math behind and a terminal script; Although it doesn't work as expected these days, the concept is actually quite nice if you're studying a different language. I'll try to implement on at least two different languages just to gain more experience with it.

About
--------
[I just found about about something interesting](http://www.pc-help.org/obscure.htm) and thought of making a generator of it.  I'm quite dumb, so if you are as dumb as me, I can try to explain what happens behind the scene.
For example, let's use http://example.com/ as an example; if you click on it, of course it's going to redirect to the site, right? But what about this: http://1572395042@1572395042 
And no, it's not just a 404 page.

If we the following dig command 

```console
foo@bar:~$ dig example.com
```

we'll get  something like this:

<img src="/img/1.png" width="45%" height="45%">


 And if you look closely, on the ANSWER SECTION, it's showing us example.com's ipv4 address, which is ``93.184.216.34``, and if you put it into your browser, it goes to the same 404 page. Regardless of what I say, let me show you this is the real IP address of example.com with curl:

<img src="/img/3.png" width="45%" height="45%">


So, what does that mean? Why is example.com's ipv4 and this weird url http://1572395042@1572395042  redirecting to the same page, which is exampe.com? Let's break it in parts to make more understandable.

## How it works

Anything can go between **http://** and **@**, doesn't matter what you put, it won't affect the final result at all, It'll be redirected to https://example.com's ipv4. 
For example: 
- http://google.com@1572395042 
- http://github.com@1572395042

This is actually for authentication, if a webpage asks for a username/password, you can login just by putting it into the url.
Example:
- http://anything:whatever@example.com

But what about the number after the **@**? Well, it's the same as the ipv4, it is just being expressed in a different way. There are some ways to express the ipv4 address
- dword
	- **double word** because it consists of two binary "words" of 16 bits expressed in decimal (base 10);
- octal
	-  expressed in base 8
- hexadecimal
	- expressed in base 16

#### Dword
To get a dword is quite simple, here's the formula:
> x * 256 + y = result
> >result * 256 + m = result2
> >>result2 * 256 + n

Replacing it with `93.184.216.34` it becomes
> **93**  * 256 + **184** = result
> >result * 256 + **216** = result2
> >>result2 * 256 + **34**

Result is 1572395042.
#### Octal
It's also quite simple, all we need to do is convert each decimal IP number to an octal:
> 93 / 8 = 11,625

- Dividend minus the remainder, in this case 93 - 88 = 5;

> 11 / 8 = 1,375

- Dividend minus the remainder, in this case 11 - 8 = 3;

> 1/8 = 0,125

- Dividend minus the remainder, in this case 1 - 0 = 1;

Part of the result is the reverse, 135. Of course I'm not doing every number here, this README will become really long, thankfully there's tons of converter online out there.
Result: 013502700330042
**Obs:** Don't mind the zeros, they're just to show your browser these are in fact octal numbers. Try it out: http://0135.0270.0330.042

#### Hexadecimal
Hexadecimal is kinda a pain, so let's use a converter, I'm using [this](https://www.rapidtables.com/convert/number/hex-to-decimal.html). This leave us with 5d.b8.d8.22, being expressed as an IP address like this: http://0x5d.0xb8.0xd8.0x22

### What about the domain name?
You can convert it to a Hexadecimal too! See [http://%65%78%61%6D%70%6C%65%2E%63%6F%6D](http://%65%78%61%6D%70%6C%65%2E%63%6F%6D)!

## Conclusion
This is quite useless now days, since most sites use a webserver that doesn't allow direct access to an IP address without a header, like nginx or apache, or an updated browser which asks the user if he/she wants to login and inform about fraud, but still kinda fun to test it. It just shows how much you could fool people on the past with spam links or stealing information.
- You can even mix it!
	- [http://microsoft.com@%65%78%61%6D%70%6C%65%2E%63%6F%6D](http://%65%78%61%6D%70%6C%65%2E%63%6F%6D)
	- http://google.com@1572395042
	- http://1572395042@0x5db8d822
	- http://0135.0xb8.0330.0x22
	- http://93.0270.216.0x22
	- http://youtube.com@0x5d.184.0330.042
