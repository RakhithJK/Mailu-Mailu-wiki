Draft to brainstorm about the possibility of a new best practices configuration page. IF we want to continue with this and finish it, we will add it to the documentation and remove this wiki page.

## Goal: 
The Mailu solution is very complex. Most people don't know how to correctly configure Mailu. This page should inform you:
- What are the relevant settings?
- How-To change these settings. Per relevant subject describe how to configure it.
- best practices? This is what we advise! 
- MAIN GOAL. Everything should be as easy to understand as possible. if it is too difficult, do not include it.

# Structure of page itself. How we want it to be.
- General overview (or short list) of all features.
- How-To tweak/configure each security related setting/topic.
- Maybe best practices? What do we advise?

# How to	
- postfix
  - /etc/postfix/tls_policy.map > overrides/postfix/tls_policy 
  - maybe fingerprinting could be added as well (need nextgens help for this)
- fail2ban (move this from the FAQ to this page)
- set mta-sts (domain must be configured as well with domain entry)
- ability spoof emails. Set wildcards/aliases (in gui). 


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

# Move this part to the general features page  (https://mailu.io/master/)
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
	