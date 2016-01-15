# About

The mysql-backup Docker image will provide you a container to backup a [MySQL](https://hub.docker.com/_/mysql/) or [MariaDB](https://hub.docker.com/_/mariadb/) database container.

The backup is made with [mydumper](http://centminmod.com/mydumper.html), a fast MySQL backup utility.

# Usage

To backup a [MySQL](https://hub.docker.com/_/mysql/) or [MariaDB](https://hub.docker.com/_/mariadb/) container you simply have to run a container from this Docker image and link a MySQL or MariaDB container to it.

The container will automatically detect the linked database container and tries to backup the database based on the environment variables of the database container:

* `<CONTAINER>_ENV_MYSQL_DATABASE`
* `<CONTAINER>_ENV_MYSQL_ROOT_PASSWORD`

Please note the backup will be written to `/backup` by default, so you might want to mount that directory from your host.

## Example Docker CLI client

To create the backup container via `docker` CLI client you've to run:

```bash
docker run --name my-backup --link my-mysql -v /var/mysql_backups:/backup -d confirm/mysql-backup
```

The container will stop automatically as soon as the backup has finished.
To create more backups in the future simply start your container again:

```bash
docker start my-backup
```

## Example Docker Compose

Here's an example of a [Docker Compose](https://docs.docker.com/compose/) file, e.g. `docker-compose.yml`:

```yaml
backup:
    image: confirm/mysql-backup
    volumes:
        - ./data/backup:/backup
    links:
        - my-mysql
    restart: never
```

# Configuration

## Backup path

By default the backup directory `/backup` is used.
However, you can overwrite that by setting the following environment variable:

* `BACKUP_DIR`: Path of the backup directory

## UID and GID

By default the backup will be written with UID and GID `666`.
However, you can overwrite that by setting the following environment variables:

* `BACKUP_UID`: UID of the backup
* `BACKUP_GID`: GID of the backup

## mydumper options

By default `mydumper` is invoked with the `-c` option which compresses the backup.
However, you can modify the `mydumper` options by setting the following environment variable:

* `BACKUP_OPTIONS`: Options passed to mydumper