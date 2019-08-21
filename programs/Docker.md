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
docker run <image>                               # Run in foreground
docker run <image> <command>                     # Run an image and execute a command
docker run -it <image>                           # Run an image in interactive mode (eg: shell)
docker run -d <image>                            # Run in background (detached)
docker run <image>:<version>                     # Run a specific version (default is 'latest')
docker run --name <name> <image>                 # Choose a name for the container
docker -p <host port>:<container port> <image>   # Configure port forwarding
docker -p <container_port> <image>               # Expose container port in a random host port
docker -v <host_dir>:<container_dir> <image>     # Configure directory mapping (file persistence, containers are stateless by default)
```

### See current containers

```bash
docker ps                                         # List running containers
docker inspect <name|id>                          # Show details about a given container (e.g.: IP, ports)
docker logs <name|id>                             # Show container log
docker port <name|id> <container_port>            # Find which host port maps a container port
```

