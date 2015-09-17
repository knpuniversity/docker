# Tutorial Notes

### install docker

- use the toolbox
- show docker-machine
- do ls, show nothing is there
- create the "default" machine
    - docker-machine create -d virtualbox --virtualbox-memory 2048 default
    - this is done in the "Docker Quickstart Terminal"
- turn the machine on, off, run ls
- eval the docker env

- introduce the `docker` command: once you run the `docker-machine env`
    command, `docker` is sandboxed to look at that one machine.

### Run first container
- maybe show `docker run hello-world` first?

- docker run -it debian bash
    - this creates a container from this image
- image: a set of "layers" that contain all the files needed
    to make something. So debian consists of a bunch of files/directories
    - "layers" is a new term at this point 
- docker history debian

- docker run -it php:7
    - several layers now "already" exist
- docker history php:7
    - see the 2 layers are the same as debian
    - see the default command
- docker run -it php:7 'php' '-v'
- docker run -it php:7 'php' '-i'
- each "layer" is usable as its own image, just like a git sha
    - docker images just shows the "head"/latest layer for each image
    - php:7 is just an alias to the latest image id / layer
- container: execution of an image with specific configuration
    - you basically run the image (all the files/directories) against
        this big configuration
    - configuration is a JSON file stored on your docker-daemon system
    - docker inspect
    - use the "nickname" of the container
- docker inspect
    - includes the images
    - includes the default command
    - list of volumes

### what happened ?
    - pulls image layers (union FS)
    - linux kernel created an isolated process using namespaces
        - http://man7.org/linux/man-pages/man7/namespaces.7.html
    - created cgroup rules if requested
        - limit allowed memory, swap, network, disk
    - limit local root capabilities if requested
        - http://man7.org/linux/man-pages/man7/capabilities.7.html

### create a php script
    - some basic stuff

### how to access this script from within the container ?
     - use a volume
     - or copy the file in the image itself

### Dockerfile
    - add more stuff in the image
        - pdo mysql, intl, mbstring, …

### build an image
    - tag an image

### push an image to the public registry (docker hub)

### make it talk to mysql
    - links

### but but but, my command line is only 80 chars wide!

### introduce compose
    - translate command line to yaml
    - define services
    - define links between them

### run it
- didn't last long!
- compose services are meant to be long running processes
    - fpm + nginx

### to the clouds!
    - works well locally?
    - what about in a remote instance?

### rise of the machine
    - docker-machine create --driver $driver $name
    - export ENV variables
    - docker-compose up -d

### didn't work ?

That's beause your setup is not portable (host specific volumes!). Let's fix that.

    - tweak config specifically for prod
        - you want a different Dockerfile for prod and dev?
            - xdebug, COPY sources VS volumes, different fpm config
    - extend the default yaml file, one for each setup
    - don't use volumes, instead COPY the sources in the image
        - so much faster once built, since composer vendors are in the image already
            - either copy the vendors folder
            - or use composer install in a Dockerfile step

