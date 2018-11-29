# Modelica In Action

This repository contains examples that demonstrate how to
create, compile and simulate Modelica models using [JModelica.org](http://www.jmodelica.org).

## Notes

All the `make` commands mentioned in this guide reference the
[Makefile](https://github.com/mbonvini/ModelicaInAction/blob/master/Makefile)
located in the root directory of this repository.

## Installation

In order to simplify your life the repository contains all the code
necessary to create a [Docker](https://www.docker.com) container that runs JModelica.org.
In case you wonder what's a Docker container

> Docker containers wrap a piece of software in a complete filesystem that contains
> everything needed to run: code, runtime,   system tools, system libraries – anything
> that can be installed on a server. This guarantees that the software will always
> run the same, regardless of its environment (see https://www.docker.com/what-docker
> for more info).

In this case I created a "recipe", also known as [Docker file](https://github.com/mbonvini/ModelicaInAction/blob/master/docker/Dockerfile),
that describes how to build a container with installed everything that's needed
to work with JModelica.org.

Once created and started, the container and your computer will interact as shown
in the following image.

![alt tag](https://github.com/mbonvini/ModelicaInAction/blob/master/images/container_scheme.png)

Upon start, some folders included in this repository will be shared with the container,
in the meantime the container runs an IPython server. The container exposes the port
used by the IPython server to your local machine. In this way you can connect to the
IPython server in the container with a browser. Once you access the IPython
server you can start working with JModelica.org.

The Docker container is based on a so called image (something similar to a snapshot
of a virtual machine). To create the image you have two options

 * manually build the image with the command
 `make build-image`

 * download the image with the command
 `make download-image`

The second option is preferable because it doesn't require you to wait while Docker
compiles from source JModelica.org and all its dependencies.

As of November 28 2018, there are two version of the image: 1.0 and 2.0, the latter has
installed a more recent version of JModelica as well as Jupyter notebook instead of IPython.
To download the version you're interested in, add the `VERSION=x.0` to the make rules
you invoke, for example to download the image version 2.0 run

```
make download-image VERSION=2.0
```

You can verify that the container image has been built (or downloaded) using the command
`docker images`. In my case when I run the command I see

```
$ docker images
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
modelicainaction/jmodelica   2.0                 7f7f224e3da2        23 hours ago        3.1GB
ubuntu                       18.04               93fd78260bd1        9 days ago          86.2MB
ubuntu                       16.04               f753707788c5        23 months ago       127.2 MB
modelicainaction/jmodelica   1.0                 adcc4c39b0d6        23 months ago       2.7GB
```

## Starting the container

Once the container image has been created we can start it.
To start the container run the command `make start`. Again, you can choose which version
to run by specifying the version like `make start VERSION=2.0`. This command starts
the container with installed JModelica.org and an IPython notebook server
listening on port 8888. The same port used by the IPython/Jupyter server is exposed
by the container and redirected to the localhost.
This means that if you open a browser and go to http://127.0.0.1:8888 you should see something
like this

![alt tag](https://github.com/mbonvini/ModelicaInAction/blob/master/images/ipython_home.png)

By default the container is configured to share the following folders

 * `modelica` - a folder containing the source code of the Modelica models used in the examples

 * `ipynotebooks` - a folder containing the ipython notebooks with examples

Every Modelica model located in the folder `modelica` will be immediately visible
to JModelica.org. The same is true for the folder `ipynotebooks`, every notebook it contains
will be automatically visible when you open the browser and go to http://127.0.0.1:8888.

You can verify that the container is running with the command `docker ps -a`

```
$ docker ps -a
CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS              PORTS                      NAMES
639bd33a27fb        modelicainaction/jmodelica:1.0   "sh -c 'ipython noteb"   4 hours ago         Up 4 hours          127.0.0.1:8888->8888/tcp   prickly_mayer
```

and you can stop and remove the container (not the container image) with the command
`docker stop 639bd33a27fb && docker rm 639bd33a27fb` where `639bd33a27fb` is the
container id.

## NOTES

- Version 2.0 uses Jupyter instead of IPython notebooks, and it requires to prompt a password when you connect
to the server from your browser. The password is `modelicainaction`.
- I've seen some warnings and minor problems when running the examples in the most recent version 2.0.
Unfortunately JModelica.org documentation doesn't seem to be up to date and I can't do much about it.
I was still able to run simulations, but as usual use it at your own risk!