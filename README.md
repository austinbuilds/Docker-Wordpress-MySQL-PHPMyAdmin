# :whale: Docker WordPress MySQL PHPMyAdmin

A boilerplate and example of WordPress, MySQL and PHPMyAdmin with Docker. Setup Wordpress, MySQL and PHPMyadmin in seconds with a single command using Docker Compose.

## :computer: Technologies Demonstrated

- Docker
- WordPress
- MySQL
- PHPMyAdmin

## :floppy_disk: Install

```sh
docker-compose up -d
```

View at localhost:8000

PHPMyAdmin at localhost:8080

:pushpin: Wordpress includes Apache and PHP.

:pushpin: phpMyAdmin user is root

## :whale: Docker

### Docker Compose

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

Docker Compose official [documentation](https://docs.docker.com/compose/).

#### Run

```sh
docker-compose up
```

:pushpin: add -d flag to run detached

#### Stop

`Ctrl + C`

#### Down

```sh
docker-compose down
```

#### Rebuild

```sh
docker-compose build
```

### Continue Learning

More examples can be found via Dockers official [documentation](https://docs.docker.com/samples/).

## :man_technologist: Author

- **Austin** - [austinbuilds](https://github.com/austinbuilds)
