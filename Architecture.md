General architecture
====================

The mail infrastructure is based on a standard MTA-LDA pattern :

 * Postfix for incoming and outgoing emails ;
 * Rmilter as a filtering interface before delivery (with rspamd and ClamAV) ;
 * Dovecot as a delivery agent and reading (IMAP) server ;
 * Roundcube (or any Webmail) as a user-friendly Web client ;
 * Fetchmail as a client to fetch remote accounts (POP/IMAP) ;
 * Freeposte (Flask application) as an administration interface.