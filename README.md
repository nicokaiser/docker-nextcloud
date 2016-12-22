# Nextcloud Docker image with Apache and PHP 7.0

# How to use this image

## Start Nextcloud

Starting the Nextcloud instance listening on port 80 is as easy as the following:

```console
$ docker run -d -p 80:80 nicokaiser/nextcloud
```

Then go to http://localhost/ and go through the wizard. By default this container uses SQLite for data storage, but the wizard should allow for connecting to an existing database.

For a MySQL database you can link an database container, e.g. `--link my-mysql:mysql`, and then use `mysql` as the database host on setup.

## Persistent data

All data beyond what lives in the database (file uploads, etc) is stored within the default volume `/var/www/html`. With this volume, Nextcloud will only be updated when the file `version.php` is not present.

-   `-v /<mydatalocation>:/var/www/html`

For fine grained data persistence, you can use 3 volumes, as shown below.

-   `-v /<mydatalocation>/apps:/var/www/html/apps` installed / modified apps
-   `-v /<mydatalocation>/config:/var/www/html/config` local configuration
-   `-v /<mydatalocation>/data:/var/www/html/data` the actual data of your Nextcloud

## ... via [`docker-compose`](https://github.com/docker/compose)

Example `docker-compose.yml` for `nextcloud`:

```yaml
# Nextcloud with MariaDB/MySQL
#
# Access via "http://localhost:8080" (or "http://$(docker-machine ip):8080" if using docker-machine)
#
# During initial Nextcloud setup, select "Storage & database" --> "Configure the database" --> "MySQL/MariaDB"
# Database user: root
# Database password: example
# Database name: pick any name
# Database host: replace "localhost" with "mysql"

version: '2'

services:

  nextcloud:
    image: nicokaiser/nextcloud
    ports:
      - 8080:80

  mysql:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: example
```
