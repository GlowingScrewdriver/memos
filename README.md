# Memos - Open Source, Self-hosted, Your Notes, Your Way

<img align="right" height="96px" src="https://www.usememos.com/logo-rounded.png" alt="Memos" />

An open-source, self-hosted note-taking solution designed for seamless deployment and multi-platform access. Experience effortless plain text writing with pain-free, complemented by robust Markdown syntax support for enhanced formatting.

-------
## About this fork
This fork attempts to allow Memos to run under a subpath of the host server. The motive
behind this effort is to be able to run Memos behind a reverse proxy that multiplexes
based on the HTTP path prefix rather than the domain name.

At present, this fork of Memos serves the frontend and the API endpoints under the `/memos/`
subpath of its server (i.e. at http://example.com/memos/ instead of http://example.com/).


### Reverse HTTP proxy configuration
The reverse proxy must be configured to catch requests to `/memos/` and forward
them _unaltered_ to the memos server.

The following examples assume Memos is listening on port 8081.

#### Caddy example (this goes in your [Caddyfile](https://caddyserver.com/docs/caddyfile))
```Caddyfile
handle /memos/* {
    reverse_proxy :8081
}
```

#### Nginx example (this goes in your [nginx.conf](https://nginx.org/en/docs/example.html))
_Note: I do not use the Nginx setup anymore, although I once did. Use at your own risk._
```nginx
http {
    server {
        # Memos
        location /memos/ {
                proxy_pass http://127.0.0.1:8081/memos/;
        }
    }
}
```

### Known issues:
* Sometimes, when visiting http://example.com/memos/, Memos fails to load, with the following error
(or similar) on the webpage:
  ```
  Unexpected Application Error!
  r is undefined
  ```
  However, visiting http://example.com/memos/explore and then navigating from there works just fine.

### Future scope:
* Configurable subpath, rather than hardcoded

---------

For more information about Memos that isn't specific to this fork, refer the [upstream's](https://github.com/usememos/memos) README. Enjoy!
