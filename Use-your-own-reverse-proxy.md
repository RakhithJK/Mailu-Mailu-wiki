One of Mailu use cases is as part of a larger services platform, where maybe other Web services are available than the Webmail and admin interface of Mailu.

In such a configuration, one would usually run a frontend reverse proxy to serve all Web contents based on criteria like the requested hostname (virtual hosts) and/or the requested path. Mailu Web frontend is disabled in the default setup for security reasons, it is however expected that most users will enable it at some point. Also, due to Docker Compose configuration structure, it is impossible for us to make disabling the Web frontend completely available through a configuration variable. This guide was written to help users setup such an architecture.

There are basically three options, from the most to the least recommended one:
- have Mailu Web frontend listen locally and use your own Web frontend on top of it
- override Mailu Web frontend configuration
- disable Mailu Web frontend completely and use your own

All options will require that you modify the ``docker-compose.yml`` file.

Have Mailu Web frontend listen locally
======================================

The simplest and safest option is to modify the port forwards for Mailu Web frontend and have your own frontend point there. For instance, in the ``http`` section of Mailu ``docker-compose.yml``, use local ports 8080 and 8443 respectively for HTTP and HTTPS:

```
  http:
    # build: $FRONTEND
    image: mailu/$FRONTEND:$VERSION
    restart: always
    env_file: .env
    ports:
      - "127.0.0.1:8080:80"
      - "127.0.0.1:8443:443"
    volumes:
      - "$ROOT/certs:/certs"
```

Then on your own frontend, point to these local ports. In practice, you only need to point to the HTTPS port (as the HTTP port simply redirects there). Here is an example Nginx configuration: 

```
server {
  listen 443;
  server_name mymailhost.tld;

  # [...] here goes your standard configuration

  location / {
    proxy_pass https://localhost:8443;
  }
}
```

Because the admin interface is served as ``/admin`` and the Webmail as ``/webmail`` you may also want to use a single virtual host and serve other applications (still Nginx): 

```
server {
  # [...] here goes your standard configuration

  location /webmail {
    proxy_pass https://localhost:8443;
  }

  location /admin {
    proxy_pass https://localhost:8443;
  }

  location /main_app {
    proxy_pass https://some-host;
  }

  location /other_app {
    proxy_pass https://some-other-host;
  }

  location /local_app {
    root /path/to/your/files;
  }

  location / {
    return 301 $scheme://$host/main_app;
  }
}
```

Finally, you might want to serve the admin interface on a separate virtual host but not expose the admin container directly (have your own HTTPS virtual hosts on top of Mailu, one public for the Webmail and one internal for administration for instance).

Here is an example configuration : 

```
server {
  listen <public_ip>:443;
  server_name yourpublicname.tld;
  # [...] here goes your standard configuration

  location /webmail {
    proxy_pass https://localhost:8443;
  }
}

server {
  listen <internal_ip>:443;
  server_name yourinternalname.tld;
  # [...] here goes your standard configuration

  location /admin {
    proxy_pass https://localhost:8443;
  }

}
```