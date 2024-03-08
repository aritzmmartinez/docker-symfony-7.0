# 🐳 Docker + Symfony 7.0 + PHP 8.2 + MySQL + Nginx

## Description
This is a complete stack for running Symfony 7.0 into Docker containers using docker-compose tool.

It is composed by 4 containers:

- `nginx`, acting as the webserver (web).
- `php`, the PHP-FPM container with the 8.2 version of PHP (be).
- `db` which is the MySQL database container with a **MySQL 8.0** image (db).

## Installation
1. Clone this repo.
2. If you haven’t already, you’ll need to install Docker and Docker Compose.
3. Before creating the containers, you must adapt the names of the services in the docker.compose.yml and where they are referenced, as well as change the environment variables of the '.env'
4. Thanks to the Makefile file, we can run the 'make run' command to initialize the docker containers.
5. Once the containers have been built, you will need to run 'composer install' inside the php container to install the dependencies.

Good luck with your project! 🍀
