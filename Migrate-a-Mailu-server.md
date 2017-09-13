This guide consists in a simple procedure to migrate a Mailu instance from one server to a new server, while avoiding the loss of any email. Some additional thoughts suggest ways of avoiding any downtime for client users.

# Concepts

The SMTP protocol has an embedded retry mechanism and multiple MX that can serve a single domain, so that most migration process or maintenance process do not require any specific care.

Mailu relys heavily on files for storing everything, which helps the migration process, that can be performed based on file synchronization.

# Migration process

The suggested migration process consists in setting up a new backup server that drops incoming emails (Mailu not started), synchronizing both servers, stopping the main server and launching the backup server. Then, the backup server is switched as a main MX and the old server is deleted.

1. Prepare your new server, copy your `docker-compose.yml`, `.env` and basic configuration files to the server, so that it is ready to start configuration Mailu, *do not start Mailu*
2. Setup your DNS so that the backup server is an additional, deprioritized MX for the domain; this can be complex if you serve many domains, in which case you can simply accept that some remote MX will retry for a couple minutes, skip this step
3. While your DNS TTL expires and your modification propagates, start *rsyncing* your Mailu directory (`data`, `dkim`, `mail`, etc.) to the new server, repeat until there are only a couple files synchronized
4. Stop Mailu on the old server and run a final `rsync` while no process is writing to the files
5. Start Mailu on the new server, and production should be back to normal
6. Set your new server as the main MX for your domains, if you did not set an additional MX, make sure you still change the `A` and `AAAA` record for your MX name.

# Additional thoughts

TODO