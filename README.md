# docker-eclipse

Eclipse v4.4.1 in a Docker container

## Requirements

* Docker 1.2+ (should work fine on 1.0+ but I haven't tried)
* An X11 socket

## Quickstart

Assuming `$HOME/bin` is on your `PATH` and that you are able to run `docker`
commands [without `sudo`](http://docs.docker.io/installation/ubuntulinux/#giving-non-root-access),
you can use the [provided `eclipse` script](eclipse) to start a disposable
Eclipse Docker container with your project sources mounted at `/home/developer/workspace`
within the container:

```sh
# The image size is currently 1.131 GB, so go grab a coffee while Docker downloads it
docker pull fgrehm/eclipse:v4.4.1
L=$HOME/bin/eclipse && curl -sL https://github.com/fgrehm/docker-eclipse/raw/master/eclipse > $L && chmod +x $L
cd /path/to/java/project
eclipse
```

Once you close Eclipse the container will be removed and no traces of it will be
kept on your machine (apart from the Docker image of course).

## Making plugins persist between sessions

Eclipse plugins are kept on `$HOME/.eclipse` inside the container, so if you
want to keep them around after you close it, you'll need to share it with your
host.

For example:

```sh
mkdir -p .eclipse-docker
docker run -ti --rm \
           -e DISPLAY=$DISPLAY \
           -v /tmp/.X11-unix:/tmp/.X11-unix \
           -v `pwd`/.eclipse-docker:/home/developer \
           -v `pwd`:/workspace \
           fgrehm/eclipse:v4.4.1
```

## Help! I started the container but I don't see the Eclipse screen

You might have an issue with the X11 socket permissions since the default user
used by the base image has an user and group ids set to `1000`, in that case
you can run either create your own base image with the appropriate ids or run
`xhost +` on your machine and try again.
