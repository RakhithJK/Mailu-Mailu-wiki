The demo server is for demonstration and test purposes only. Please be respectful and keep the demo server functional for others to be able to try it out.

The server is reset every day at 3am, french time. If you find the server is unusable, you can still ask for someone to reset it manually on Freenode, channel ``#mailu`. Please do not open tickets everytime the server is down.

Keep in mind that the demo server is also used for some automated tests and runs the latest unstable version. If you find actual bugs when using the demo server, please report these!

Connecting to the server
========================

 * Server name : ``test.freeposte.io``
 * IP address : ``163.172.216.78``
 * Webmail : https://test.freeposte.io
 * Admin UI : https://test.freeposte.io/admin
 * Admin login : ``admin@test.freeposte.io``
 * Admin password : ``letmein``
 * Docker UI : http://163.172.216.78:9000

Adding domains
==============

If you wish to add new domains to the server for test purposes, you could either direct the MX for one of you domains to ``test.freeposte.io``.

Also, all subdomains of ``test.freeposte.io`` point to this server. Thus, you can simply add ``foo.test.freeposte.io`` and ``bar.test.freeposte.io`` for your tests.
