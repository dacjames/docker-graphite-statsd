



## Quick Start

```sh
```

This starts a Docker container named: **graphite**

```sh
sudo docker run -d \
  --name graphite \
  -p 8080:80 \
  -p 2003:2003 \
  -v /local/path/to/.htpasswd:/etc/nginx/.htpasswd \
  sitespeedio/graphite
```

### Includes the following components

* [Nginx](http://nginx.org/) - reverse proxies the graphite dashboard
* [Graphite](http://graphite.readthedocs.org/en/latest/) - front-end dashboard
* [Carbon](http://graphite.readthedocs.org/en/latest/carbon-daemons.html) - back-end
* [Statsd](https://github.com/etsy/statsd/wiki) - UDP based back-end proxy

### Mapped Ports

Host | Container | Service
---- | --------- | -------------------------------------------------------------------------------------------------------------------
  80 |        80 | [nginx](https://www.nginx.com/resources/admin-guide/)
2003 |      2003 | [carbon](https://graphite.readthedocs.org/en/latest/feeding-carbon.html)
2004 |      2004 | [carbon aggregator](https://graphite.readthedocs.org/en/latest/carbon-daemons.html#carbon-aggregator-py)
2023 |      2023 | [carbon pickle](https://graphite.readthedocs.org/en/latest/feeding-carbon.html#the-pickle-protocol)
2024 |      2024 | [carbon aggregator pickle](https://graphite.readthedocs.org/en/latest/feeding-carbon.html#the-pickle-protocol)
8125 |      8125 | [statsd](https://github.com/etsy/statsd/blob/master/docs/server.md)
8126 |      8126 | [statsd admin](https://github.com/etsy/statsd/blob/v0.7.2/docs/admin_interface.md)

**Note**: You can override the default port mappings by specifying them when starting the container.

```sh
docker run -d\
 --name graphite\
 --restart=always\
 -p 80:80\
 -p 2003-2004:2003-2004\
 -p 2023-2024:2023-2024\
 -p 8125:8125/udp\
 -p 8126:8126\
 hopsoft/graphite-statsd
```

### Mounted Volumes

Host              | Container                  | Notes
----------------- | -------------------------- | -------------------------------
DOCKER ASSIGNED   | /opt/graphite/conf         | graphite config
DOCKER ASSIGNED   | /opt/graphite/storage      | graphite stats storage
DOCKER ASSIGNED   | /etc/nginx                 | nginx config
DOCKER ASSIGNED   | /opt/statsd                | statsd config
DOCKER ASSIGNED   | /etc/logrotate.d           | logrotate config
DOCKER ASSIGNED   | /var/log                   | log files

### Base Image

Built using [Phusion's base image](https://github.com/phusion/baseimage-docker).

* All Graphite related processes are run as daemons & monitored with [runit](http://smarden.org/runit/).
* Includes additional services such as logrotate.

## Start Using Graphite & Statsd

### Send Some Stats

```sh
```

TODO also map log dirs


The default one looks like this:

```
retentions = 5m:1d,15m:21d,30m:60d
```

It will store data for 2 months, change that if you need to store data longer. Etsy has good [documentation](https://github.com/etsy/statsd/blob/master/docs/graphite.md) on how to setup your Graphite metrics.

To change it, you can feed the image with a new *storage-schemas.conf*. The one you want to replace is located  
*/opt/graphite/conf/storage-schemas.conf*


Simply specify the desired volumes when starting the container.


**Note**: The container will initialize properly if you mount empty volumes at
          `/opt/graphite`, `/opt/graphite/conf`, `/opt/graphite/storage`, or `/opt/statsd`

Built using [Phusion's base image](https://github.com/phusion/baseimage-docker).

