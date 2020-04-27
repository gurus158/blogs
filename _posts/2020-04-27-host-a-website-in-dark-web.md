---
published: true
---
## How to host a website in Dark web from localhost

### Hi 

Most of you might heard of dark-web. [darkWeb-wiki](https://en.wikipedia.org/wiki/Dark_web) This post is about how one can host own website in darkweb.

Lets Start the journey from localhost to dark-web

But First, When I say dark web website , what I mean is a **.onion** website. Dark web is made by onion websites, which are run through encrypted network to gain anonimity. And onion sites can't be open using naive browser like chrome. To access .onion site we required Tor browser.

**Step 1**

Install tor in system.

Command: **apt-get install tor**

![Screenshot from 2020-04-27 13-20-09.png]({{site.baseurl}}/_posts/Screenshot from 2020-04-27 13-20-09.png)


**Step 2**

Create a website and host in localhost.

For that i create a demo index.html page and host it at 127.0.0.1:80 using apache

![Screenshot from 2020-04-27 13-25-54.png]({{site.baseurl}}/_posts/Screenshot from 2020-04-27 13-25-54.png)

save this at **/var/www/html/** as index.html

start apache server using command: **service apache2 start**

Check 127.0.0.1 in browser to see if index.html get hosted correctly

![Screenshot from 2020-04-27 13-29-45.png]({{site.baseurl}}/_posts/Screenshot from 2020-04-27 13-29-45.png)


**Step 3**

Now we need to tell tor about our localhost website so that it can map it to dark-net.

For that we need to edit **torrc** file located at **/etc/tor/torrc**.
open this file at locate the lines 
- **HiddenServiceDir /var/lib/tor/hidden_service/**
- **HiddenServicePort 80 127.0.0.1:80***

and uncomment them as shown in image
![Screenshot from 2020-04-27 13-38-42.png]({{site.baseurl}}/_posts/Screenshot from 2020-04-27 13-38-42.png)

**Step 4** 
Most of the part is done.. last step is to start tor service and get the domain for our website.

start tor using command : **service tor start**

Tor generate a random 56 chars domain with .onion extension for our hidden service.

To get the domain check the file 
**/var/lib/tor/hidden_service/hostname**
![Screenshot from 2020-04-27 13-43-15.png]({{site.baseurl}}/_posts/Screenshot from 2020-04-27 13-43-15.png)

so onion url for my website is **gkgzabctyvnvwtyblnjqdgxhcfkwqscy5j5yeuoxadfwudm2eplvc5yd.onion**

Now i just have to open this url in Tor browser while continuing hosting my localhost

![Screenshot from 2020-04-27 13-45-10.png]({{site.baseurl}}/_posts/Screenshot from 2020-04-27 13-45-10.png)

So this is how we can host our own website at dark-web.