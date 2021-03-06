Software Collection rh-mysql56 Dockerfile
=========================================

How to build this Dockerfile
----------------------------

Building this Dockerfile requires a Red Hat Enterprise Linux 7 host
system with Software Collections entitlements available.

To build the Dockerfile, run:

```
# cd rh-mysql56
# docker build -t=rh-mysql56 .
```


MySQL 5.6 Docker Image based on rh-mysql56 Software Collection
==============================================================

MySQL 5.6 is a multi-user, multi-threaded SQL database server. It is a
client/server implementation consisting of a server daemon (mysqld)
and many different client programs and libraries. This docker image contains
the MySQL 5.6 server and some accompanying files and directories.

The Docker image includes packages from Software Collections (SCL).
For more information about Software Collections, see
http://softwarecollections.org.


Environment variables and volumes
----------------------------------

The image recognizes following environment variables that you can set during
initialization, by passing `-e VAR=VALUE` to the Docker run command.

|    Variable name       |    Description                            |
| :--------------------- | ----------------------------------------- |
|  `MYSQL_USER`          | User name for MySQL account to be created |
|  `MYSQL_PASSWORD`      | Password for the user account             |
|  `MYSQL_DATABASE`      | Database name                             |
|  `MYSQL_ROOT_PASSWORD` | Password for the root user (optional)     |

Either MYSQL_ROOT_PASSWORD or MYSQL_USER, MYSQL_PASSWORD and MYSQL_DATABASE
must be specified.

Following environment variables influence MySQL configuration file. They are all optional.

|    Variable name                |    Description                                                    |    Default
| :------------------------------ | ----------------------------------------------------------------- | -------------------------------
|  `MYSQL_LOWER_CASE_TABLE_NAMES` | Sets how the table names are stored and compared                  |  0
|  `MYSQL_MAX_CONNECTIONS`        | The maximum permitted number of simultaneous client connections   |  151
|  `MYSQL_FT_MIN_WORD_LEN`        | The minimum length of the word to be included in a FULLTEXT index |  4
|  `MYSQL_FT_MAX_WORD_LEN`        | The maximum length of the word to be included in a FULLTEXT index |  20
|  `MYSQL_AIO`                    | Controls the `innodb_use_native_aio` setting value in case the native AIO is broken. See http://help.directadmin.com/item.php?id=529 |  1

You can also set following mount points by passing `-v /host:/container` flag
to docker.

|  Volume mount point      | Description          |
| :----------------------- | -------------------- |
|  `/var/lib/mysql/data`   | MySQL data directory |


Usage
-----

To just run the dameon and not store the database in a host directory,
you need to execute the following command:

```
# docker run -d -e MYSQL_ROOT_PASSWORD=secret-password -p 3306:3306 THIS_IMAGE
```

This will run the daemon in default configuration and port 3306 will be
exposed and mapped to host.

It is recommended to use run the container with mounted data directory everytime.
This example shows how to run the container with `/host/data` directory mounted
and so the database will store data into this directory on host:

```
docker run -d -v /host/data:/var/lib/mysql/data THIS_IMAGE
```

This will create a container running MySQL 5.6 daemon
and storing data into `/host/data` on the host.

For debugging purposes or just connecting to the running container, run
`docker exec -ti CONTAINERID container-entrypoint` in a separate terminal.

You can stop the detached container by running `docker stop CONTAINERID`.


Database initialization
-----------------------

If the database directory is not initialized, the container script will first
run [`mysql_install_db`](https://dev.mysql.com/doc/refman/5.5/en/mysql-install-db.html)
during start and setup necessary database users and passwords. After the database is
initialized, or if it was already present, `mysqld` is executed and will run as PID 1.

To pass arguments that are used for initializing the database if it is not yet
initialized, define them as environment variables

```
docker run -d -e MYSQL_USER=user -e MYSQL_PASSWORD=pass -e MYSQL_DATABASE=db THIS_IMAGE
```

This will create a container running MySQL 5.6 with database
`db` and user with credentials `user:pass` that has access to the database `db`.


MySQL root user
---------------
The root user has no password set by default, only allowing local connections.
You can set it by setting `MYSQL_ROOT_PASSWORD` environment variable when initializing
your container. This will allow you to login to the root account remotely. Local
connections will still not require password.



General container help
----------------------

Run `docker run THIS_IMAGE container-usage` to get this help.

Run `docker run -ti THIS_IMAGE bash` to obtain interactive shell.

Run `docker exec -ti CONTAINERID container-entrypoint` to access already running container.

In order to get the container ID after running the image, pass `--cidfile=`
option to the `docker run` command. That will instruct Docker to write
a file with the container ID.

You may try `-e CONT_DEBUG=VAL` with VAL up to 3 to get more verbose debugging
info.


Report bugs to <http://bugzilla.redhat.com>.




