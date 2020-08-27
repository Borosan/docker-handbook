# Docker Compose cheat sheet

### Basic config example

```text
# docker-compose.yml
version: '3'

services:
  web:
    build: .
    # build from Dockerfile
    context: ./Path
    dockerfile: Dockerfile
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: redis
```

### Common commands

* [docker-compose start](https://docs.docker.com/compose/reference/start/) Starts existing containers for a service.
* [docker-compose stop](https://docs.docker.com/compose/reference/stop/) Stops running containers without removing them.
* [docker-compose pause](https://docs.docker.com/compose/reference/pause/) Pauses running containers of a service.
* [docker-compose unpause](https://docs.docker.com/compose/reference/unpause/) Unpauses paused containers of a service.
* [docker-compose ps](https://docs.docker.com/compose/reference/ps/) Lists containers.
* [docker-compose up](https://docs.docker.com/compose/reference/up/) Builds, \(re\)creates, starts, and attaches to containers for a service.
* [docker-compose down](https://docs.docker.com/compose/reference/down/) Stops containers and removes containers, networks, volumes, and images created by up.

### Config file reference

#### Building

```text
web:
  # build from Dockerfile
  build: .
  # build from custom Dockerfile
  build:
    context: ./dir
    dockerfile: Dockerfile.dev
  # build from image
  image: ubuntu
  image: ubuntu:14.04
  image: tutum/influxdb
  image: example-registry:4000/postgresql
  image: a4bc65fd
```

#### Ports

```text
ports:
  - "3000"
  - "8000:80"  # guest:host
# expose ports to linked services (not to host)
expose: ["3000"]
```

#### Commands

```text
# command to execute
command: bundle exec thin -p 3000
command: [bundle, exec, thin, -p, 3000]

# override the entrypoint
entrypoint: /app/start.sh
entrypoint: [php, -d, vendor/bin/phpunit]
```

#### Environment variables

```text
# environment vars
environment:
  RACK_ENV: development
environment:
  - RACK_ENV=development

# environment vars from file
env_file: .env
env_file: [.env, .development.env]
```

#### Dependencies

```text
# makes the `db` service available as the hostname `database`
# (implies depends_on)
links:
  - db:database
  - redis

# make sure `db` is alive before starting
depends_on:
  - db
```

#### Other options

```text
# make this service extend another
extends:
  file: common.yml  # optional
  service: webapp
volumes:
  - /var/lib/mysql
  - ./_data:/var/lib/mysql
```

### Advanced features

#### Labels

```text
services:
  web:
    labels:
      com.example.description: "Accounting web app"
```

#### DNS servers

```text
services:
  web:
    dns: 8.8.8.8
    dns:
      - 8.8.8.8
      - 8.8.4.4
```

#### Devices

```text
services:
  web:
    devices:
      - "/dev/ttyUSB0:/dev/ttyUSB0"
```

#### External links

```text
services:
  web:
    external_links:
      - redis_1
      - project_db_1:mysql
```

#### Hosts

```text
services:
  web:
    extra_hosts:
      - "somehost:192.168.1.100"
```

#### Network

```text
# creates a custom network called `frontend`
networks:
  frontend:
```

#### External network

```text
# join a preexisting network
networks:
  default:
    external:
      name: frontend
```

.

.

.

----

[https://gist.github.com/jonlabelle/bd667a97666ecda7bbc4f1cc9446d43a](https://gist.github.com/jonlabelle/bd667a97666ecda7bbc4f1cc9446d43a)

[https://devhints.io/docker-compose](https://devhints.io/docker-compose)

.

