# Service details

Describe for each service (image):
* Purpose of service and general description.
* General description of all features it offers and what technologies are used. 
* How is it started (initialized).
* How is the configuration managed.
* Explanation of file structure for each service in the project. 

List of all services (image name / technical name )
services:
* Admin / Admin
* Imap /Dovecot
* Front/ Nginx
* Smtp / Postfix
* Antispam / Rspamd
* Antivirus Clamav
* fetchmail / fetchmail
* Webdav radicale
* traefik-certdumper
* Resolver / unbound
* rainloop / nloop
* roundcube/ roundcube
* docs / docs
* setup / setup

## Admin / Admin

Docker Image Name: admin
* Technical Name: admin
* Location: /core/admin
* General purpose: Provides the web-based administration interface and handles all non-mail tasks.
* General features: Longer bullet point list of features.
* Volume mapping: bullet point list of mapping to /mailu folder.
* Overrides: bullet point list of override folder
* Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

## Imap / Dovecot
* Docker Image Name: imap
* Technical Name: dovecot
* Location: /core/dovecot
* General purpose: Provides IMAP/POP3 access (access to emails).
* General features: 
  * Provides IMAP/POP3 access (access to emails).
  * Authentication is handled by Front/Admin
  * Provides sieve functionality?
  * Marks email as spam/ham when email is move to/from Junk/Spam folder.
* Volume mapping:
  * "/mailu/mail:/mail" - contains all email boxes
* Overrides: 
  * "/mailu/overrides/dovecot/dovecot.conf:/overrides/dovecot.conf:ro" - Used to add/override settings in dovecot.conf
* Connectivity: 
  * imap/dovecot > admin
    * uses podop to retrieve settings from admin (which are set in administration web interface). Via address http://admin/internal/dovecot
  * Imap/Dovecot > Antispam/Rspamd
    * Dovecot reports via rspamc (rspam client) to rspamd HAM and SPAM. Used for bayes learning.
  * smtp/postfix > imap/dovecot
    * Messages received by postfix are transported to dovecot.(LMTP: imap:2525)

## Front / Nginx
* Docker Image Name: front
* Technical Name: nginx
* Location: /core/nginx
* General purpose: The main entry point (reverse proxy) for Mailu. Also handles SSL certificates.
* General features: Longer bullet point list of features.
* Volume mapping: bullet point list of mapping to /mailu folder.
* Overrides: bullet point list of override folder
* Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

## Smtp / Postfix
* Docker Image Name: smtp
* Technical Name: postfix
* Location: /core/postfix
* General purpose: Provides the SMTP server for receiving emails.
* General features: 
  * Provides SMTP capabilities.
  * Authentication is handled via Front/Admin
  * Fully automatically configured via Web Administration interface.
  * MTA-STS (SMTP MTA Strict Transport Security)
  * DANE validation.
* Volume mapping:
  * "/mailu/mailqueue:/queue" - contains all received emails that must still be processed (sent to imap server or sent to other email servers).
* Overrides: bullet point list of override folder
  * - "/mailu/overrides/postfix:/overrides:ro":
    * all postfix config files (https://mailu.io/master/faq.html#how-can-i-override-settings)
    * overrides/postfix/tls_policy.map - overrides MTA-STS. E.g. can be used to temporarily downgrade security for a destination that has incorrectly configured (failing) MTA-STS policy / DANE.
* Connectivity: 
  * Front/Nginx > smtp/postfix
    * Front proxies postfix
  * Front/Nginx > admin > smtp/postfix
    * front authenticates any users via admin, before proxying the connection to postfix.
  * smtp/postfix > admin
    * uses Podop to retrieve configuration from Admin via http://admin/internal/postfix/
    * This allows postfix to retrieve all setting configured in the web administration gui for postfix.
  * smtp/postfix > antispam/rspamd
    * Uses rspamd as milter. 
      * For received emails, ask rspamd to accept/reject the email.
      * For received emails for sending from clients, ask rspamd to sign the email (dkim/arc).
  * smtp/postfix > imap/dovecot
    * Messages received by postfix are transported to dovecot.(LMTP: imap:2525)

## Antispam / Rspamd
* Docker Image Name: antispam
* Technical Name: rspamd
* Location: /core/rspamd
* General purpose: Provides spam filtering.
* General features: 
  * Has authentication. Front/Admin authenticates user before providing access to rspamd.
  * Antivirus checking of emails (uses clamav for this)
  * Spam checking via many different metrics. See https://rspamd.com/features.html for a list.
  * ARC signing
  * DKIM signing
  * learning via ham/spam
  * Very neat Web UI.
* Volume mapping:
  * "/mailu/filter:/var/lib/rspamd" - contains rspamd settings (made in web ui) and bayes classifying (users marking email as spam/ham).     
* Overrides: 
  * "/mailu/overrides/rspamd:/etc/rspamd/override.d:ro" - contains overrides for rspamd configuration. 
* Connectivity: 
  * Front/Nginx > Antispam/Rspamd
    * Front proxies Antispam.
    * Front authenticates the user (via admin) before providing access to Rspamd.
  * Smtp/Postfix > Antispam/Rspamd
    * Uses rspamd as milter (should I accept this email or reject it...), but also other things.
      * antivirus (does the email contain a virus)
      * spam check (is this email spam?)
      * ARC signing
      * Dkim signing
  * Antispam/Rspamd > Antivirus/Clamav
    * Uses clamav to scan email for viruses.
  * Imap/Dovecot > Antispam/Rspamd
    * Dovecot reports via rspamc (rspam client) to rspamd HAM and SPAM. Used for bayes learning.
  * Antispam/Rspamd > Redis
    * Stores history in Redis. Perhaps also other things I'm not aware of.

## Antivirus / Clamav
* Docker Image Name: antivirus
* Technical Name: clamav
* Location: /optional/clamav
* General purpose: provides antivirus scanning for emails and email attachments.
* General features: 
  * Clamav automatically updates itself using freshclam.
  * Scans incoming and outgoing emails for viruses.
* Volume mapping: 
  * "/mailu/filter:/data" - contains downloaded virus definitions.
* Overrides: None
* Connectivity: 
  * Postfix -> Rspamd -> Clamav
    * Postfix uses Rspamd as milter. Rspamd connects to Clamav for scanning emails for viruses.

## Fetchmail / Fetchmail
* Docker Image Name: fetchmail
* Technical Name: fetchmail
* Location: /optional/fetchmail
* General purpose: Allows for fetching (pulling) emails from other email accounts.
* General features:
  * Fetches emails from other email accounts into the user's Mailu email account.
  * Fully configured via Web Administration interface.
  * Web Administration interface shows results
    * DateTime of last fetch.
    * Any error messages.
* Volume mapping:
  * "/mailu/data/fetchmail:/data" - is used for storing a file that keeps track of fetched emails.
* Overrides: None
* Connectivity: 
  * Fetchmail > Admin
    * Fetchmail retrieves (GET) from http://admin/internal/fetch the email inboxes that must be fetched.
    * Fetchmail submits (POST) the results of the last operation to http://admin/internal/fetch.

## Webdav / Radicale
* Docker Image Name: webdav
* Technical Name: radicale
* Location: /optional/radicale
* General purpose: Provides address book and calendar management via webdav.
* General features: 
  * (Basic) Authentication is handled by Front/Admin.
  * Provides address book via webdav for each Mailu user.
  * Provides calendar management via webdav for each Mailu user.
* Volume mapping: 
  * "/mailu/dav:/data" - contains all data of radicale for each user.
* Overrides: None
* Connectivity: 
  * Front -> Radicale
    * Front handles (basic) authentication and provides the username via HTTP_X_USER header to Radicale.

## Traefik-certdumper / Traefik-certdumper
* Docker Image Name: traefik-certdumper
* Technical Name: traefik-certdumper
* Location: /optional/traefik-certdumper
* General purpose: Helper image to dump Traefik certificates to Mailu.
* General features: 
  * Helper image to dump Traefik certificates to Mailu.
* Volume mapping: 
  * /data/traefik:/traefik" - folder which contains acme.json from traefik
  * "/mailu/certs:/output" - folder where to dump certificates for mailu
* Overrides: None
* Connectivity: None

## Resolver / Unbound
* Docker Image Name: Resolver
* Technical Name: Unbound
* Location: /optional/unbound
* General purpose: Provides dedicated DNS server
* General features: 
  * DNS caching for all services. E.g. Rspamd will not get rate limited for spamming realtime blackhole lists. It will receive the cached response from the resolver/unbound.
  * DNSSEC?
  * DNS Root lookups
  * Other things?? Ask other contributors who know more about this.
* Volume mapping: None
* Overrides: None
* Connectivity: 
  * Other services services connect to Resolver/Unbound for DNS resolution. These services are:
    * front / nginx
    * smtp / postfix
    * antispam / rspamd
    * antivirus / clamav
    * fetchmail

## Rainloop / Rainloop
* Docker Image Name: rainloop
* Technical Name: rainloop
* Location: location in project
* General purpose: Provides webmail access for Mailu email accounts
* General features: 
  * Provides webmail access for Mailu email accounts.
  * Authentication is fully integrated with Mailu SSO. Uses Mailu SSO page for authentication.
  * Sieve scripts management. 
  * Has no database. I think all user configuration is stored locally in the Internet Browser storage.
* Volume mapping:
  * /mailu/webmail to /data
    * Basically contains all rainloop config files.
* Overrides: None
* Connectivity: 
  * Front > Rainloop
    * Front proxies Rainloop. Users connect via Front/Nginx to Rainloop.
  * Rainloop > redirects internet browser to Front 
    * For user authentication, Rainloop redirects the user (internet browser) to the SSO page to login.
    * If the user is authenticated and tries to access webmail, a one time token is generated.
    * Username and one time token are passed via HTTP headers to Rainloop.
    * Rainloop uses the username and one time token for connecting to Imap/dovecot.
  * Rainloop > Imap
    * Connect as IMAP client to imap/dovecot.

## roundcube
* Docker Image Name: Image name
* Technical Name: Main Image technology / software
* Location: /webmails/roundcube
* General purpose: Provides webmail access for Mailu email accounts
* General features: 
  * Provides webmail access for Mailu email accounts.
  * Authentication is fully integrated with Mailu SSO. Uses Mailu SSO page for authentication.
  * Sieve scripts management. 
  * Has a database. All user options are stored in the database.
* Volume mapping:
  * /mailu/webmail to /data
    * Basically contains all roundcube config files and SQLite database files (Contacts and Address book).
* Overrides: None
* Connectivity: 
  * Front > Roundcube
    * Front proxies roundcube. Users connect via Front/Nginx to Roundcube.
  * Roundcube > redirects internet browser to Front 
    * For user authentication, Roundcube redirects the user (internet browser) to the SSO page to login.
    * If the user is authenticated and tries to access webmail, a one time token is generated.
    * Username and one time token are passed via HTTP headers to Rainloop.
    * Roundcube uses the username and one time token for connecting to Imap/dovecot.
  * Roundcube > Imap
    * Connect as IMAP client to imap/dovecot.

## Docs / Docs
* Docker Image Name: docs
* Technical Name: docs
* Location: /docs
* General purpose: Hosts documentation of Mailu project.
* General features:
  * Provides documentation of Mailu
  * Offers documentation for each version of Mailu. Versioning is coupled to the branch of the git project. E.g. Branch 1.8 is used for the documentation of Mailu 1.8. Version master shows documentation of branch master.
  * Uses (reStructuredText)[https://docutils.sourceforge.io/rst.html] as its markup language.
* Core technologies:
  * Sphinx - documentation web framework.
* Volume mapping: None
* Overrides: None
* Connectivity: None

## Setup / Setup
* Docker Image Name: Image name
* Technical Name: Main Image technology / software
* Location: /setup
* General purpose: Setup wizard website for creating docker-compose.yml and mailu.env file.
* General features:
  * Provides a Wizard that guides you in configuration all relevant settings for Mailu.
  * Creates the docker-compose.yml and mailu.env files with results of completing the wizard.
  * Provides first steps for starting your Mailu installation.
* Core technologies:
  * Flask - Python web framework.
* Volume mapping: None.
* Overrides: None.
* Connectivity: 
  * Setup > redis
    * Is used for temporary storage of generated files.


# Architecture
* location to describe how certain general features work in detail. E.g.
  * authentication architecture
  * integration of antispam in rspamd (instead of as milter)
  * how dkim/dmarc/arc is handled
  * admin that provides access to data stored in the database for all other services.
  * Podop
  * Other concepts?
  * etc
* Picture that shows how all services are linked to each other
  * also describe what kind of communication we have between all services.
  * We can create this as soon the service descriptions are done.