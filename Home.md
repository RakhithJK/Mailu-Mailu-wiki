**The documentation was migrated to the Mailu Website : https://mailu.io.** \
**For user support, please reach out to us on our Mailu channel on Matrix: https://matrix.to/#/#mailu:tedomum.net**

Here You will only find developer docs. For the time being everything is still WIP. Nothing is set in stone. Take everything with a grain of salt until this line is removed. \
\
\

Todo
==

Describe for each service (image):
* Purpose of service and general description.
* General description of all features it offers and what technologies are used. 
* How is it started (initialized).
* How is the configuration managed.
* Explanation of file structure for each service in the project. 

Architecture:
* location to describe how certain general features work in detail. E.g.
  * authentication architecture
  * integration of antispam in rspamd (instead of as milter)
  * how dkim/dmarc/arc is handled
  * admin that provides access to data stored in the database for all other services.
  * Podop
  * etc
* Picture that shows how all services are linked to each other
  * also describe what kind of communication we have between all services.

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

Admin / Admin
==
Docker Image Name: admin
Technical Name: admin
Location: /core/admin
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Imap / Dovecot
==
Docker Image Name: imap
Technical Name: dovecot
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Front / Nginx
==
Docker Image Name: front
Technical Name: nginx
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Smtp / Postfix
==
Docker Image Name: smtp
Technical Name: postfix
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Antispam / Rspamd
==
Docker Image Name: antispam
Technical Name: rspamd
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Antivirus / Clamav
==
Docker Image Name: antivirus
Technical Name: clamav
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Fetchmail / Fetchmail
==
Docker Image Name: fetchmail
Technical Name: fetchmail
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Webdav / Radicale
==
Docker Image Name: webdav
Technical Name: radicale
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Traefik-certdumper / Traefik-certdumper
==
Docker Image Name: traefik-certdumper
Technical Name: traefik-certdumper
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Resolver / Unbound
==
Docker Image Name: Resolver
Technical Name: Unbound
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Rainloop / Rainloop
==
Docker Image Name: rainloop
Technical Name: rainloop
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

roundcube
==
Docker Image Name: Image name
Technical Name: Main Image technology / software
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Docs / Docs
==
Docker Image Name: docs
Technical Name: docs
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.

Setup / Setup
==
Docker Image Name: Image name
Technical Name: Main Image technology / software
Location: location in project
General purpose: how would you describe it in one sentence?
General features: Longer bullet point list of features.
Volume mapping: bullet point list of mapping to /mailu folder.
Overrides: bullet point list of override folder
Connectivity: Bullet point list to what other images it connects, for what purpose, and the direction. I guess this means it is also a dependency overview.
