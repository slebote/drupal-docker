# About this Repo

**This image is based on drupal docker official image :**

- Docker image : [drupal](https://hub.docker.com/_/drupal/)
- Github repository : [docker-library/drupal](https://github.com/docker-library/drupal)

**Additions :**

- HTTPS support : usage of self-signed snakeoil certificates (based on linux package `ssl-cert`)
- Clean permissions with docker : force `www-data` user to every mounted volumes
- Mysql client for drush support

## Supported tags and respective `Dockerfile` links

- `7.58-apache`, `7-58` [(7/apache/Dockerfile)](https://github.com/slebote/drupal-docker/blob/master/7/apache/Dockerfile)
- `7.59-apache`, `7-59` [(7/apache/Dockerfile)](https://github.com/slebote/drupal-docker/blob/master/7/apache/Dockerfile)
- `7.61-apache`, `7-61` [(7/apache/Dockerfile)](https://github.com/slebote/drupal-docker/blob/master/7/apache/Dockerfile)
- `8.6.3-apache`, `8.6.3` [(8/apache/Dockerfile)](https://github.com/slebote/drupal-docker/blob/master/8/apache/Dockerfile)

## Docker compose

Exemple of docker-compose version 3 implementation using [drupal-composer/drupal-project](https://github.com/drupal-composer/drupal-project)
composer template (this template implements `vendor` directory with `drush` command).

### Docker compose with your own proxy

```
version: "3"
services:
  web:
    image: slebote/drupal-docker:7.61
    volumes:
      - ./web/sites:/var/www/html/sites
      - ./web/modules:/var/www/html/modules
      - ./web/profiles:/var/www/html/profiles
      - ./web/themes:/var/www/html/themes
      - ./vendor:/var/www/html/vendor
    working_dir: /var/www/html
    links:
      - db
    ports:
      - 443
    environment:
      - PATH=/var/www/html/vendor/bin:$PATH
    networks:
      - webnet
  db:
    image: mysql:5.7
    volumes:
      - mysql-datas:/var/lib/mysql
    ports:
      - 3306
    environment:
      - MYSQL_USER=my-awesome-project
      - MYSQL_PASSWORD=my-awesome-project
      - MYSQL_DATABASE=my-awesome-project_development
      - MYSQL_ROOT_PASSWORD=my-awesome-project_root
    networks:
      - webnet
networks:
  webnet:
volumes:
  mysql-datas:
```

### Docker compose with [jwilder/nginx-proxy](https://hub.docker.com/r/jwilder/nginx-proxy/) auto-proxy

If you are using this nginx auto-proxy don't forget to add certificates as volume where you will run the container : `-v {your_certificates_directory}:/etc/nginx/certs`

```
version: "3"
services:
  web:
    image: slebote/drupal-docker:7.61
    volumes:
      - ./web/sites:/var/www/html/sites
      - ./web/modules:/var/www/html/modules
      - ./web/profiles:/var/www/html/profiles
      - ./web/themes:/var/www/html/themes
      - ./vendor:/var/www/html/vendor
    working_dir: /var/www/html
    links:
      - db
    ports:
      - 32001:443
    environment:
      - VIRTUAL_HOST=my-awesome-project.local.fr
      - VIRTUAL_PORT=443
      - VIRTUAL_PROTO=https
      - PATH=/var/www/html/vendor/bin:$PATH
    networks:
      - webnet
  db:
    image: mysql:5.7
    volumes:
      - mysql-datas:/var/lib/mysql
    ports:
      - 32001:3306
    environment:
      - MYSQL_USER=my-awesome-project
      - MYSQL_PASSWORD=my-awesome-project
      - MYSQL_DATABASE=my-awesome-project_development
      - MYSQL_ROOT_PASSWORD=my-awesome-project_root
    networks:
      - webnet
networks:
  webnet:
volumes:
  mysql-datas:
```