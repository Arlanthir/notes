# Docker

Docker allows developers to wrap software and dependencies, building images that can be instantiated, running in isolated containers.  
This means self-configured software that always runs, not only "on my machine".

## Searching for images

Browse and search via browser in https://hub.docker.com/ or use the following command:

```bash
docker search <software name>
```

## Running images

### Start a container

```bash
docker run <image>                                   # Run in foreground
docker run <image> <command>                         # Run an image and execute a command
docker run -it <image>                               # Run an image in interactive mode (eg: shell)
docker run -d <image>                                # Run in background (detached)
docker run <image>:<version>                         # Run a specific version (default is 'latest')
docker run --name <name> <image>                     # Choose a name for the container
docker run -p <host port>:<container port> <image>   # Configure port forwarding
docker run -p <container_port> <image>               # Expose container port in a random host port
docker run -v <host_dir>:<container_dir> <image>     # Configure directory mapping (file persistence, containers are stateless by default)
docker run -e NODE_ENV=production <image>            # Set environment variables
```

### See current containers

```bash
docker ps                                         # List running containers
docker inspect <name|id>                          # Show details about a given container (e.g.: IP, ports)
docker logs <name|id>                             # Show container log
docker port <name|id> <container_port>            # Find which host port maps a container port
```

### Stop a container

```bash
docker kill <name|id>
```

## Building images

### Dockerfile

```bash
FROM nginx:alpine                   # Starting point
COPY . /usr/share/nginx/html        # Copy local folder `.` to container folder.
RUN <command>                       # Runs a shell command
EXPOSE 80 433 7000-8000             # Configure ports to be bound to
CMD ["nginx", "-g", "daemon off;"]  # The default command and arguments to run when instantiated
```

**Good practice**: use particular versions (instead of latest) and update them manually periodically, to ensure compatibility with no surprises.

**Cache**: Each copy compares the new files to the old ones. If they are different, the following lines are cache invalidated and run again. Otherwise, cached values will be used. For this reason, it's important to split each copy into the most correct line and be careful with their order.

**ONBUILD prefix**: Any instructions prefixed with `ONBUILD` will only run when the image is being used as a base for another image.

### Build an image

```bash
docker build -t <name>:<version> .       # -t To tag with a friendly name and version (example: webserver-image:v1)
```

### List built images

```bash
docker images
```


