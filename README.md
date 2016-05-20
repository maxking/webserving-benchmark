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


## Running the client

You can use any bencharmking client to run tests on this server out of [many
available][1], most of which are available for fee. I used [Autobench][2] to run
generate load (i.e. requests to the server) to actually benchmark the
server. You can also download and compile the source from [here][3].

		 autobench --single_host --host1 localhost --uri1 /index.php --quiet \
          --low_rate 20 --high_rate 200 --rate_step 20 --num_call 10 \
          --num_conn 5000 --timeout 5 --file results.tsv

The above command will generate connections starting from 20/sec to 200/sec with
a stop of 20 and send in 10 requests per connections till a total of 5000
connections.

It will spit out a nice tsv (tab seperate values, like csv) that you can import
to excel or google sheets to plot. Although along with Autobench there is also a
script called `bench2graph` that you can use to create plots using gnuplot.


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

[1]: https://en.wikipedia.org/wiki/Web_server_benchmarking
[2]: http://www.xenoclast.org/autobench/man/autobench.html
[3]: https://github.com/menavaur/Autobench
