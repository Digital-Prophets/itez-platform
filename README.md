# ITEZ-Platform

All ITEZ services started from a single repository

*Keep in mind this repository is for local development only and is not meant to be deployed on any production environment!.*

## Requirements
1. [Docker](https://docs.docker.com/install/)
2. [Docker Compose](https://docs.docker.com/compose/install/)


## How to run it?

1. Clone the repository:

```
$ git clone https://github.com/Digital-Prophets/itez-platform.git --recursive --jobs 2
```

2. We are using shared folders to enable live code reloading. Without this, Docker Compose will not start:
    - Windows/MacOS: Add the cloned `itez-platform` directory to Docker shared directories (Preferences -> Resources -> File sharing).
    - Windows/MacOS: Make sure that in Docker preferences you have dedicated at least 5 GB of memory (Preferences -> Resources -> Advanced).
    - Linux: No action required, sharing already enabled and memory for Docker engine is not limited.

3. Go to the cloned directory:
```
$ cd itez-platform
```

4. Build the application:
```
$ docker-compose up --build
```

5. Apply Django migrations:
```
$ docker-compose run --rm api python manage.py makemigrations
$ docker-compose run --rm api python manage.py migrate
```

6. Collect static files:
```
$ docker-compose run --rm api python manage.py collectstatic --noinput
```

7. To create the admin super user:
```
$ docker-compose run --rm api python manage.py createsuperuser
```

8. Run the application:
```
$ docker-compose up
```
*Both the frontend and api backend services will build and start up the necesarry services. If nothing shows up on port 3000 wait until `Compiled successfully` shows in the console output.*


## How to update the subprojects to the newest versions?
This repository contains newest stable versions.
When new release appear, pull new version of this repository.
In order to update all of them to their newest versions, run:
```
$ git submodule update --remote
```

You can find the latest version of API backend and the Frontend in their individual repositories:

- https://github.com/Digital-Prophets/itez
- https://github.com/Digital-Prophets/itez-frontend

## How to solve issues with lack of available space or build errors after update

Most of the time both issues can be solved by cleaning up space taken by old containers. After that, we build again the whole platform. 


1. During these problems. Make sure docker stack is not running
```
$ docker-compose stop
```

2. Remove existing volumes

**Warning!** Proceeding will remove also your database container! If you need existing data, please remove only services which cause problems! https://docs.docker.com/compose/reference/rm/
```
docker-compose rm
```

3. Build fresh containers 
```
docker-compose build
```

4. Now you can run fresh environment using commands from `How to run it?` section. Done!

### Still no available space

If you are getting issues with lack of available space, consider prunning your docker cache:

**Warning!** This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache 
  
  More info: https://docs.docker.com/engine/reference/commandline/system_prune/
  
<details><summary>I've been warned</summary>
<p>

```
$ docker system prune
```

</p>
</details>

## How to run application parts?
  - `docker-compose up api worker` for backend services only
  - `docker-compose up` for backend and frontend services


## Where is the application running?
- Itez Core (API) - http://localhost:8000
- Itez Frontend - http://localhost:3000
- Jaeger UI (APM) - http://localhost:16686
- Mailhog (Test email interface) - http://localhost:8025 


## License

Disclaimer: Everything you see here is open and free to use as long as you comply with the [license](https://github.com/Digital-Prophets/itez-platform/blob/main/LICENSE). There are no hidden charges. We promise to do our best to fix bugs and improve the code.

#### Crafted with ❤️ by [DigitalProphets](http://DigitalProphets.com)
