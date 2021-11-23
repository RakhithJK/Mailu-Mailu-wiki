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
* General features: Longer bullet point list of features.
* Volume mapping: bullet point list of mapping to /mailu folder.
* Overrides: bullet point list of override folder
* Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a * dependency overview.

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
* General features: Longer bullet point list of features.
* Volume mapping: bullet point list of mapping to /mailu folder.
* Overrides: bullet point list of override folder
* Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

## Antispam / Rspamd
* Docker Image Name: antispam
* Technical Name: rspamd
* Location: /core/rspamd
* General purpose: Provides spam filtering.
* General features: Longer bullet point list of features.
* Volume mapping: bullet point list of mapping to /mailu folder.
* Overrides: bullet point list of override folder
* Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

## Antivirus / Clamav
* Docker Image Name: antivirus
* Technical Name: clamav
* Location: /optional/clamav
* General purpose: provides antivirus scanning for emails and email attachments.
* General features: Longer bullet point list of features.
* Volume mapping: bullet point list of mapping to /mailu folder.
* Overrides: bullet point list of override folder
* Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

## Fetchmail / Fetchmail
* Docker Image Name: fetchmail
* Technical Name: fetchmail
* Location: /optional/fetchmail
* General purpose: Allows for fetching (pulling) emails from other email accounts.
* General features: Longer bullet point list of features.
* Volume mapping: bullet point list of mapping to /mailu folder.
* Overrides: bullet point list of override folder
* Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

## Webdav / Radicale
* Docker Image Name: webdav
* Technical Name: radicale
* Location: /optional/radicale
* General purpose: Provides address book and calendar management via webdav.
* General features: Longer bullet point list of features.
* Volume mapping: bullet point list of mapping to /mailu folder.
* Overrides: bullet point list of override folder
* Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

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
  * Rainloop > Front
    * Connects to Front to authenticate the current user.
    * If the current user is not authenticates, Rainloop redirect the user to the SSO login page of admin.
    * If the current is is authenticated, the user is logged on.
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
  * Roundcube > Front
    * Connects to Front to authenticate the current user.
    * If the current user is not authenticates, roundcube redirect the user to the SSO login page of admin.
    * If the current is is authenticated, the user is logged on.
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