# Docker image of Fluentd with GELF output plugin

Official fluentd image is used as a base image plus GELF output plugin is included. It uses TCP connection to Graylog2 server by default, so that log entries should not be lost due to network outages.

It is based on ideas/code from 
* [fluentd-gelf-docker][1]
* [fluent-plugin-gelf][2]

## What is Fluentd?

Fluentd is an open source data collector, which lets you unify the data
collection and consumption for a better use and understanding of data.

> [www.fluentd.org](http://www.fluentd.org/)

![Fluentd Logo](http://www.fluentd.org/assets/img/miscellany/fluentd-logo.png)

## Image versions

This image is based on the popular [Alpine Linux project][3], available in
[the alpine official image][4].
Alpine Linux is much smaller than most distribution base images (~5MB), and
thus leads to much slimmer images in general.

### References

[1]: https://github.com/onmomo/fluentd-gelf-docker/
[2]: https://github.com/emsearcy/fluent-plugin-gelf/
[3]: http://alpinelinux.org
[4]: https://hub.docker.com/_/alpine

## Build docker image

Based on the provided Dockerfile you can build and customize the docker image.

```
docker build -t qiiq/fluentd-gelf .
```

## Running docker image

First run fluentd container

```
docker run --name fluentd -p 127.0.0.1:24224:24224 -p 127.0.0.1:24224:24224/udp -e GRAYLOG2_HOST=graylog.host -e GRAYLOG2_PORT=12201 qiiq/fluentd-gelf
```

Then you can start other containers with fluentd log driver:

```
docker run -d  --log-driver=fluentd --log-opt fluentd-address=127.0.0.1:24224 --log-opt fluentd-async-connect=true alpine sh
```
