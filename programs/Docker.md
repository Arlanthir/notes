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

Data containers are containers that only store data.

```bash
docker create -v /config --name dataContainer busybox   # Create data container
docker cp config.conf dataContainer:/config/            # Copy a file to the container
docker export dataContainer > dataContainer.tar         # Backup the data container
docker import dataContainer.tar                         # Restore the container

# To use the data in another container:
docker run --volumes-from dataContainer ubuntu ls /config   # Example to run ubuntu and list the files in the volume
```

## Labels

```bash
docker run -l mylabel=myvalue <image>       # Label the container with a mylabel label, with the value myvalue
docker run --label-file=mylabels <image>    # Read labels from a file (each label in a line)
docker ps --filter "label=user=scrapbook"        # Filter running containers by label
docker images --filter "label=vendor=Katacoda"   # Filter images by label
```

**Good practice**: Labels subject to third-party tooling should use a reverse-DNS format (e.g. `com.katacoda.version`).

## Networks

```bash
docker network create <network-name>
docker run --net=<network-name> <image>       # Register a container in a network to communicate with others in that network
docker network connect <network> <container>  # Connect an existing container to a network
docker network connect --alias <alias> <network> <container>  # Connect an existing container to a network, assigning it an alias
docker network ls                                 # List configured networks
docker network inspect <network>                  # Get more details from a network (including connected containers)
docker network disconnect <network> <container>   # Disconnect container from a network
```

## Load balancing

Use `nginx-proxy` to balance the load on containers automatically.

```bash
docker run -d -p 80:80 -e DEFAULT_HOST=proxy.example -v /var/run/docker.sock:/tmp/docker.sock:ro --name nginx jwilder/nginx-proxy
docker run -d -p 80 -e VIRTUAL_HOST=proxy.example katacoda/docker-http-server # For each desired container to balance
```

## Docker Compose

Manage starting, configuration, variables, links, of multiple containers (orchestration).  
The configuration is stored in a `docker-compose.yml` file.

```yml
web:                       # Container name
  build: .                 # docker build .
  links:
    - redis
  ports:
    - "3000"
    - "8000"
redis:                     # Another container
  image: redis:alpine      # Based on an image (not built)
  volumes:
    - /var/redis/data:/data
```

The full documentation can be found at https://docs.docker.com/compose/compose-file/ .

### Running the containers

```bash
 docker-compose up -d                       # Start all containers in detached mode
 docker-compose up <container>              # Start only one of the containers
 docker-compose up --scale <container>=3    # Scale up or down depending on container name
```

### Managing the running containers

```bash
docker-compose ps       # Lists running containers of this compose configuration
docker-compose logs     # Merges all logs
docker-compose stop     # Stops all containers
docker-compose start    # (Re)starts all containers
docker-compose rm       # Removes all stopped containers
```

