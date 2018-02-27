---
layout: post
title: HEIST
subtitle: Everything you need to know to secure against this attack
image: img/heist.png
published: true
---
### What is HEIST
Heist stands for (HTTP Encrypted Information can be Stolen through TCP-windows). This attack can be used to steal your SSN/Emails from any website across the Internet without requiring any kind of access to your network. So should you be worried? Yes.

I am going to talk about how this vulnerability affects us, and how you can protect your customers & yourself from this kind of attack. 

### HEIST Explained:
HEIST is a new attack vector based on previous CRIME and BREACH SSL exploits used to steal information from secure websites. It was presented in Blackhat 2016 by Mathy Vanhoef & Tom Van Goethe. HEIST represents a very real threat to millions of sites across the web that rely on HTTPS for site security. Even big names such as Google, Facebook, or Salesforce could potentially be at risk. 

<img src="/img/Screenshot from 2016-08-04 15-25-17.png" width="50%" />

As its name suggests, the HEIST technique—short for HTTP Encrypted Information can be Stolen Through TCP-Windows—works by exploiting the way HTTPS responses are delivered over TCP (Transmission Control Protocol) to servers, thus compromising essential communications. 

Interestingly, a common behavior of web browsers can make this kind of attack successful. Suppose we have two websites: www.1.com and www.2.com. If I embed www.2.com inside my page as an iframe / advertisement, then www.2.com--based on response time--can predict the response time TTL and other parameters used in TCP. For example, REST API with valid URL containing sensitive information is typically secure under TLS, but a boolean-based or dictionary-based evaluation (depending on the scenario) can reveal changes in payload size response time and TCP frames delivery. Although the information is still encrypted, it's possible to start guessing this data and eventually evaluate a valid URL or search query. This information can then be used by the attacker to perform a HEIST attack. Because browsers send authentication cookies (e.g. when you log in to facebook.com) from any request coming from your browser. Those requests are authenticated. 


<img src="/img/Screen Shot 2016-08-04 at 12.50.48 PM.png" width="50%" />

That's also the challenge here as for the browser to separate those kind of request and also on server side dropping packets with requests that might be initiated by a separate domain.

<img src="" width="/img/Screen Shot 2016-08-04 at 12.52.22 PM.png" />

Gone in 30 seconds: New attack plucks secrets from HTTPS-protected pages

Once attackers know the size of an encrypted response, they are free to use one of two previously devised exploits to ferret out the plaintext contained inside it. Both the BREACH and the CRIME exploits are able to decrypt payloads by manipulating the file compression that sites use to make pages load more quickly

### Attack requirements:
This attack does not require Man-in-the-Middle access, so the number of possible attack vectors are nearly endless.
- Any page with an embedded advertisement can send requests to your server.
- Any page with an embedded iframe containing your website can send requests to your server.
- Any non-malicious link which executes JavaScript in your browser is a game over for you. There's not much you can do if you are exposed to the attack via a link that you received and opened in email or from Facebook.


### Impact:
- Use HEIST to exploit BREACH/CRIME
- Extract CSRF tokens, private message content
- Only 2 requirements: gzip/SSL compression + reflected content 
- Obtain sensitive content from web services
- Response size is related to user (victim) state


### Protection:
There is very little you can do to protect yourself as of today. One of the simplest things you can get started on is to begin using private browsing in Firefox/Chrome. By doing so, you have disabled the third-party cookies which, for example, Firefox disables in private browsing mode. It does not offer 100% protection, but you will at least secure yourself against a decent portion of potential attack vectors. 
- As explained by Firefox. 

Note: This functionality was added to prevent user tracking not to prevent HEIST but does help you a little bit.

### Firefox tracking protection

> Accept third-party cookies:
Always: Firefox will always accept cookies from http://site2.com when you are visitinghttp://site1.com.
From Visited: If you have visited http://site2.com previously, Firefox will accept cookies from this site when you are visiting http://site1.com, otherwise Firefox will not accept them.
Never: Firefox will never accept cookies from http://site2.com when you are visitinghttp://site1.com. For more information, see Disable third-party cookies in Firefox to stop some types of tracking by advertisers.
Keep until:
they expire: If selected, Firefox will allow the sites you visit to specify how long Firefox should keep their cookies.
I close Firefox: If selected, your cookies will be deleted when you close Firefox.

Now you can bypass the 'From' visited options here if you have already visited Facebook. So using the 'Never' option is an ideal choice here.

One the server side you need to implement JavaScript logic in your website to prevent any XHR request/call not coming directly from within your domain. If so don't sent the Cookie with it with the request. Variable response time/Drop packet can be implemented on server side to prevent this kind of attack. 


### What will not protect you ?

1. SOP (Same Origin Policy)
2. CORS Policies/Restrictions
3. Any new TLS cert/CA authority or company selling you new secure TLS