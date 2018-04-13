# Drupal docker image

This image is based on drupal docker official image :

- Docker image : [drupal](https://hub.docker.com/_/drupal/)
- Github repository : [docker-library/drupal](https://github.com/docker-library/drupal)

Additions :

- HTTPS support : usage of snakeoil certificates (linux package `ssl-cert`)
- Clean permissions with docker

## Supported tags and respective `Dockerfile` links

- `7.58-apache`, `7-58` [(7/apache/Dockerfile)](https://github.com/slebote/drupal-docker/blob/master/7/apache/Dockerfile)