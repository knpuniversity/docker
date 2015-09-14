# Tutorial Notes

### install docker
- distrib package or standalone static binary
- run the daemon
    - which IPs range to assign
    - which image storage filesystem ? (overlayFS, devicemapper, …)

### Run first container
    - php:7 on debian

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
