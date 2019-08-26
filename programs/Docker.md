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
docker run -v <host_dir>:<container_dir>:ro <image>  # Read-only volume
docker run --volumes-from <container> <image>        # Use the volumes mapping from another container
docker run -e NODE_ENV=production <image>            # Set environment variables
docker run --link <other container>:<alias> <image>  # Create host <alias> in the new container, connecting to another
docker run --log-driver=syslog <image>               # Redirect logs to syslog
docker run --log-driver=none <image>                 # Disables logging
docker run --restart=always <image>                  # Restart the image if it fails
docker run --restart=on-failure:3 <image>            # Attempt to restart the image 3 times if it fails
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
ADD app.tar.gz /src/app             # Like COPY, but also supports extracting *.tar.gz files and remote URLs
RUN <command>                       # Runs a shell command
EXPOSE 80 433 7000-8000             # Configure ports to be bound to
CMD ["nginx", "-g", "daemon off;"]  # The default command and arguments to run when instantiated
LABEL vendor=Katacoda \ com.katacoda.version=0.0.5 \ com.katacoda.build-date=2016-07-01T10:47:29Z    # Labels
```

**Good practice**: use particular versions (instead of latest) and update them manually periodically, to ensure compatibility with no surprises.

**Cache**: Each copy compares the new files to the old ones. If they are different, the following lines are cache invalidated and run again. Otherwise, cached values will be used. For this reason, it's important to split each copy into the most correct line and be careful with their order.

**OnBuild prefix (DEPRECATED)**: Any instructions prefixed with `ONBUILD` will only run when the image is being used as a base for another image. Thus, NodeJS can provide an "OnBuild" image that will only copy the `/src/app`, `package.json` and run the `npm install` on your specific build.

### .dockerignore

Similar syntax to .gitignore, ignores files from being copied to the image. Useful for sensitive files or big unneeded files.

### Build an image

```bash
docker build -t <name>:<version> .       # -t To tag with a friendly name and version (example: webserver-image:v1)
```

### List built images

```bash
docker images
```

## Data containers

```bash
docker create -v /config --name dataContainer busybox
docker cp config.conf dataContainer:/config/
docker export dataContainer > dataContainer.tar
docker import dataContainer.tar
```

## Networks

```bash
docker network create <network-name>
docker run --net=<network-name> <image>       # Register a container in a network to communicate with others in that network
docker network connect <network> <container>  # Connect an existing container to a network
docker network connect --alias <alias> <network> <container>  # Connect an existing container to a network, assigning it an alias
docker network ls
docker network inspect <network>
docker network disconnect <network> <container>
```

## Labels
```bash
docker run -l mylabel=myvalue <image>       # Label the container with a mylabel label, with the value myvalue
docker run --label-file=mylabels <image>    # Read labels from a file (each label in a line)
docker ps --filter "label=user=scrapbook"
docker images --filter "label=vendor=Katacoda"
```

**Good practice**: Labels subject to third-party tooling should use a reverse-DNS format (e.g. `com.katacoda.version`).


