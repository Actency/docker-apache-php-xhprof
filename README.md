# About this repo

This is the Actency Docker **Drupal** optimized images for apache-php with xhprof.

Available tags are:
- 5.6, latest ([5.6/Dockerfile](https://github.com/Actency/docker-apache-php-xhprof/tree/master/5.6/Dockerfile))
- 5.5 ([5.5/Dockerfile](https://github.com/Actency/docker-apache-php-xhprof/tree/master/5.5/Dockerfile))
- 5.4 ([5.4/Dockerfile](https://github.com/Actency/docker-apache-php-xhprof/tree/master/5.4/Dockerfile))

The image basically contains:

- All php libraries needed for Drupal (gd, mbstring, mcrypt, zip, soap, pdo_mysql, mysqli, xsl, opcache, calendar)
- Xhprof profiling tool which will store profiling data of every page you browse in a mongodb container


See the [Docker Hub page](https://hub.docker.com/r/actency/docker-apache-php-xhprof/) for the full readme on how to use this and for other information.
