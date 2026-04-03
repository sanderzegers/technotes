# Docker Cheatsheet

### Terminology

| Term         | Definition                                               |
| ------------ | -------------------------------------------------------- |
| Image        | read-only template used to create containers             |
| Container    | running instance of an image                             |
| Dockerfile   | file with instructions to build a Docker image           |
| Registry     | Service that stores and distributes images               |
| Repository   | A collection of image versions in a registry             |
| Volume       | Persistent storage used by containers                    |
| Bind Mount   | direct link between a host folder and a container folder |
| Port Mapping | Exposes a container port to the host machine             |
| Composer     | A tool for defining and running multi-container apps     |
| Daemon       | background Docker service managing containers and images |



### Images

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `docker pull <image>` | Download an image from a registry | `docker pull nginx` |
| `docker images` | List local images | `docker images` |
| `docker rmi <image>` | Remove an image | `docker rmi nginx` |
| `docker build -t <name>:<tag> <path>` | Build an image from a Dockerfile | `docker build -t myapp:latest .` |
| `docker tag <image> <new_name>` | Tag an image with a new name | `docker tag myapp:latest myrepo/myapp:v1` |
| `docker push <image>` | Push an image to a registry | `docker push myrepo/myapp:v1` |
| `docker image inspect <image>` | Show detailed image information | `docker image inspect nginx` |

### Containers

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `docker run <image>` | **Create** and start a container | `docker run nginx` |
| `docker run -it <image> <shell>` | Start an interactive container | `docker run -it ubuntu bash` |
| `docker run -d <image>` | Run container in background | `docker run -d nginx` |
| `docker ps` | List running containers | `docker ps` |
| `docker ps -a` | List all containers | `docker ps -a` |
| `docker start <container>` | Start a stopped container | `docker start mycontainer` |
| `docker stop <container>` | Stop a running container | `docker stop mycontainer` |
| `docker restart <container>` | Restart a container | `docker restart mycontainer` |
| `docker rm <container>` | Remove a container | `docker rm mycontainer` |
| `docker rename <old> <new>` | Rename a container | `docker rename web1 web-prod` |

### Logs and Inspection

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `docker logs <container>` | Show container logs | `docker logs mycontainer` |
| `docker logs -f <container>` | Follow container logs live | `docker logs -f mycontainer` |
| `docker inspect <container>` | Show detailed container information | `docker inspect mycontainer` |
| `docker stats` | Show live resource usage | `docker stats` |
| `docker top <container>` | Show running processes in a container | `docker top mycontainer` |
| `docker diff <container>` | Show filesystem changes in a container | `docker diff mycontainer` |

### Execute and Copy

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `docker exec -it <container> <command>` | Run a command inside a running container | `docker exec -it mycontainer sh` |
| `docker cp <src> <container>:<dest>` | Copy files into a container | `docker cp app.py mycontainer:/app/` |
| `docker cp <container>:<src> <dest>` | Copy files from a container | `docker cp mycontainer:/app/log.txt .` |

### Persistence and Mounts

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `docker run --mount type=bind,src=<host-path>,dst=<container-path>` | Bind mount a host path into a container | `docker run --mount type=bind,src=$(pwd),dst=/app python` |
| `docker run -v <host-path>:<container-path>` | Short bind mount syntax | `docker run -v $(pwd):/app python` |
| `docker run --mount type=volume,src=<volume>,dst=<container-path>` | Mount a named volume for persistent data | `docker run --mount type=volume,src=pgdata,dst=/var/lib/postgresql/data postgres` |
| `docker run -v <volume>:<container-path>` | Short named volume syntax | `docker run -v pgdata:/var/lib/postgresql/data postgres` |
| `docker volume ls` | List volumes | `docker volume ls` |
| `docker volume create <name>` | Create a volume | `docker volume create data` |
| `docker volume inspect <name>` | Show volume details | `docker volume inspect data` |
| `docker volume rm <name>` | Remove a volume | `docker volume rm data` |
| `:ro` | Mount read-only | `docker run -v $(pwd):/app:ro python` |

### Storage Rules of Thumb

| Situation | Best practice | Example |
| --------- | ------------- | ------- |
| Keep source code synced from host | Bind mount | `docker run -v $(pwd):/app node` |
| Keep database files or uploads persistent | Named volume | `docker run -v pgdata:/var/lib/postgresql/data postgres` |
| Keep installed packages or tools | Build a custom image with `Dockerfile` | `docker build -t mytools .` |
| Quick snapshot of a modified container | `docker commit` (less reproducible) | `docker commit mycontainer myimage:saved` |

### Ports and Networking

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `-p <host>:<container>` | Publish a container port | `docker run -p 8080:80 nginx` |
| `docker network ls` | List networks | `docker network ls` |
| `docker network create <name>` | Create a network | `docker network create app-net` |
| `docker network inspect <name>` | Show network details | `docker network inspect app-net` |
| `docker network connect <network> <container>` | Connect a container to a network | `docker network connect app-net mycontainer` |
| `docker network disconnect <network> <container>` | Disconnect a container from a network | `docker network disconnect app-net mycontainer` |

### Cleanup

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `docker rm $(docker ps -aq)` | Remove all containers |  |
| `docker rmi $(docker images -q)` | Remove all images |  |
| `docker container prune` | Remove stopped containers | `docker container prune` |
| `docker image prune` | Remove unused images | `docker image prune` |
| `docker volume prune` | Remove unused volumes | `docker volume prune` |
| `docker network prune` | Remove unused networks | `docker network prune` |
| `docker system prune` | Remove unused Docker data | `docker system prune` |
| `docker system prune -a` | Remove all unused images too | `docker system prune -a` |

### Docker Compose

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `docker compose up` | Create and start services | `docker compose up` |
| `docker compose up -d` | Start services in background | `docker compose up -d` |
| `docker compose down` | Stop and remove services | `docker compose down` |
| `docker compose ps` | List compose services | `docker compose ps` |
| `docker compose logs` | Show compose logs | `docker compose logs` |
| `docker compose logs -f` | Follow compose logs | `docker compose logs -f` |
| `docker compose build` | Build services | `docker compose build` |
| `docker compose exec <service> <command>` | Run a command in a service container | `docker compose exec web sh` |

### Common Flags

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `-d` | Detached mode | `docker run -d nginx` |
| `-it` | Interactive terminal | `docker run -it ubuntu bash` |
| `--name <name>` | Assign container name | `docker run --name web nginx` |
| `--rm` | Remove container when it exits | `docker run --rm ubuntu echo hello` |
| `-e KEY=VALUE` | Set environment variable | `docker run -e APP_ENV=prod myapp` |
| `-p host:container` | Publish ports | `docker run -p 3000:3000 nodeapp` |
| `-v src:dest` | Mount volume or directory | `docker run -v data:/data busybox` |

### Dockerfile Instructions

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `FROM <image>:<tag>` | Set the base image for the build | `FROM python:3.12-slim` |
| `WORKDIR <path>` | Set the working directory for following instructions | `WORKDIR /app` |
| `COPY <src> <dest>` | Copy files from build context into the image | `COPY . .` |
| `RUN <command>` | Execute a command during build, usually to install packages | `RUN pip install -r requirements.txt` |
| `ENV KEY=value` | Set environment variables that persist in the image | `ENV APP_ENV=production` |
| `ARG KEY=value` | Define a build-time variable | `ARG PYTHON_VERSION=3.12` |
| `EXPOSE <port>` | Document the port the container listens on | `EXPOSE 8000` |
| `USER <name>` | Set the default user for later instructions and runtime | `USER appuser` |
| `CMD ["executable", "arg"]` | Default command when the container starts | `CMD ["python", "app.py"]` |
| `ENTRYPOINT ["executable"]` | Make the container behave like an executable | `ENTRYPOINT ["python"]` |
| `VOLUME ["/path"]` | Mark a path for external persistent data | `VOLUME ["/data"]` |
| `# Comment` | Add a comment | `# Use debian as base image` |

### Minimal Dockerfile Template

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["python", "app.py"]
```

### Dockerfile with Installed Packages

```dockerfile
FROM ubuntu:24.04
RUN apt-get update && apt-get install -y curl git
WORKDIR /workspace
CMD ["bash"]
```

### Well known base images

format `image:tag`

Avoid  latest, prefer version tags, optional sha256 digests (fixed version):

python:3.12 usually updates image to newer 3.12.x releases
python:3.12.8 is more fixed.
@sha256: is the exact image

```
# okay for quick tests
FROM nginx:latest

# better default
FROM nginx:1.27

# best for reproducible builds
FROM nginx:1.27@sha256:...
```



| Image                      | Descriptions                                            |
| -------------------------- | ------------------------------------------------------- |
| python / node / php / java | App runtime                                             |
| debian                     | good safe default                                       |
| ubuntu                     |                                                         |
| alpine                     | smallest common base, musl instead of glibc             |
| scratch                    | empty minimal base, for single statically linked binary |
| nginx / httpd              | web server / reverse proxy                              |



### Build and Run Flow

| Syntax | Description | Example |
| ------ | ----------- | ------- |
| `docker build -t <name>:<tag> .` | Build image from the current directory | `docker build -t myapp:latest .` |
| `docker build -f <file> -t <name>:<tag> <context>` | Build with a specific Dockerfile | `docker build -f docker/dev.Dockerfile -t myapp:dev .` |
| `docker run --name <name> <image>` | Start a container from the built image | `docker run --name myapp myapp:latest` |
| `docker run -p <host>:<container> <image>` | Start and publish ports | `docker run -p 8000:8000 myapp:latest` |
| `docker run -v <host>:<container> <image>` | Start with a bind mount | `docker run -v $(pwd):/app myapp:latest` |
| `docker run -v <volume>:<container> <image>` | Start with a named volume | `docker run -v data:/data myapp:latest` |

### Multi-stage Build

```dockerfile
FROM golang:1.24 AS build
WORKDIR /src
COPY . .
RUN go build -o /app

FROM debian:stable-slim
COPY --from=build /app /usr/local/bin/app
CMD ["app"]
```

### Notes

| Topic | Note |
| ----- | ---- |
| Bind mount vs volume | Bind mounts are best when the host must see the files. Volumes are best for Docker-managed persistent data. |
| Installed packages | Packages installed with `RUN` become part of the image and are available in future containers started from that image. |
| `EXPOSE` | `EXPOSE` documents ports; publish them with `-p` when running the container. |
| `ARG` vs `ENV` | `ARG` is for build-time values. `ENV` persists into containers started from the image. |
| Reproducibility | Prefer `Dockerfile` over manually changing a container and saving it later. |
