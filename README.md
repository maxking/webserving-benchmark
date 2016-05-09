## Environment variables and defaults

* __HSTS\_HEADERS\_ENABLE__

* default: not set - if set to any value the HTTP Strict Transport Security will
  be activated on SSL Channel
 
* __HSTS\_HEADERS\_ENABLE\_NO\_SUBDOMAINS__

* default: not set - if set together with __HSTS\_HEADERS\_ENABLE__ and set to
  any value the HTTP Strict Transport Security will be deactivated on subdomains


## Running

This Dockerfile is not really made for direct usage. It should be used as
base-image for your apache2 project. But you can run it anyways.

You should overwrite the _/etc/apache2/external/_ with a folder, containing your
apache2 __\*.conf__ files (VirtualHosts etc.), certs and a __dh.pem__.  _If you
forget the dh.pem file, it will be created at the first start - but this
can/will take a long time!_

    docker run -d \
    -p 80:80 -p 443:443 \
    -v <path to .>/data/:/etc/apache2/external/ \
    -v <path to .>/data/:/var/www/html \
    maxking/webserving-benchmark

## Based on

This Dockerfile is based on the
[/_/ubuntu:15.04/](https://registry.hub.docker.com/_/ubuntu/) Official Image.

## Cheat Sheet

### Creating a high secure SSL CSR with openssl

This cert might be incompatible with Windows 2000, XP and older IE Versions

    openssl req -nodes -new -newkey rsa:4096 -out csr.pem -sha256

### Creating a self-signed ssl cert

Please note, that the Common Name (CN) is important and should be the FQDN to
the secured server:

    openssl req -x509 -newkey rsa:4086 \
    -keyout key.pem -out cert.pem \
    -days 3650 -nodes -sha256
