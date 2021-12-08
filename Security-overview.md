Draft to brainstorm about the possibility of a new security page. IF we want to continue with this and finish it, we will add it to the documentation and remove this wiki page.

## Goal: 
The Mailu solution is very complex. Nobody knows what security measures are really in place. Most people don't know how to securely configure Mailu. This page should inform you:
- What security features does Mailu have?
- What are the relevant settings?
- How-To change these settings. Per relevant subject describe how to configure it.
- Maybe best practices? This is what we advise! 

# Structure of page itself. How we want it to be.
- General overview (or short list) of all features.
- How-To tweak/configure each security related setting/topic.
- Maybe best practices? What do we advise?

# Description of all  security measures in place
- dkim
- dmarc
- arc
- spf
- antivirus
- antispam
  - realtime black listing
  - bayes learning 
  - See https://rspamd.com/features.html
- Possibility to configure with fail2ban
- All postfix related stuff?
  - ?
  - ?
- all communication go through one secure front service
	- rate limiting on failed logon 
		- per ip
		- per user
	- server side session management 
	- session forgery protection
	
# How to	
- considerations for reverse dns with certificates. Using ecdsa for performance.
	- Set the headers in your reverse proxy. Configure in mailu.env what header to trust and ip adress.
- Passwords. Credential_rounds config and what about password crypt?
- postfix
	- force starttls (no cert check, encrypt)
	- force starttls (with cert check, secure?)
	- dane
	- more I forgot :(
- fail2ban (move this from the FAQ to this page)
- set mta-sts
- describe user message rate limit. How to reset this?

#1902
docker-compose exec smtp cat /etc/postfix/tls_policy.map > overrides/postfix/tls_policy
edit overrides/postfix/tls_policy as required
then recreate the smtp container

Random related things:
something you may want to document on the security page: One can use the following command from the admin container to benchmark how many rounds to configure in CREDENTIAL_ROUNDS : python3 -m timeit -n 1 -s "from passlib.hash import bcrypt_sha256" "bcrypt_sha256.using(rounds=12).hash('password')" ... tweaking rounds and the number of iterations as required

https://www.digitalocean.com/community/tutorials/how-to-configure-mta-sts-and-tls-reporting-for-your-domain-using-apache-on-ubuntu-18-04
https://aykevl.nl/apps/mta-sts/
Here's my overrides/nginx/mta-sts.conf; You also need to add mta-sts.example.com to HOSTNAMES to ensure that you get a valid certificate for it

      location @mta-sts {
return 200 "version: STSv1
mode: testing
max_age: 86400
mx: mx.example.com\r\n";
      }

      location ^~ /.well-known/mta-sts.txt {
        try_files $uri @mta-sts;
      }
	