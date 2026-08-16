# Basics

## Volume
```
docker volume create <volume name>
```

```
docker volume ls
```

```
docker volume rm <volume name>
```

Removes all unused/dangling volumes:
```
docker volume prune
```
## Build
```
docker build -t <image name>(:<tag>) .
```
## Run
Runs the container detached:
```
docker run -d --name <container name> <image name>
```

Run detached with **bind mount**:
```
docker run -d --name <container name> --mount type=bind,src=<absolute src path>,dst=<absolute dst path> <image name>
```

Run detached with **explicit volume**:
```
# docker volume create <volume name>
docker run -d --name <container name> --mount type=volume,src=<volume name>,dst=<absolute dst path> <image name>
```

Run detached with **implicit volume**:
```
docker run -d --name <container name> -v <volume name>:<absolute dst path> <image name>
```

With port:
```
docker run -d --name <container name> -p <outer port>:<inner port> <image name>
```

Accessing the container immeadiatly:
```
docker run -it --name <container name> --mount type=bind,src=<absolute src path>,dst=<absolute dst path> <image name>
```
## Start

```
docker start <container name>
```
## Ps
All running containers:
```
docker ps
```

All containers:
```
docker ps -a
```
## Exec
exec runs command in the container.

Connecting via shell:
```
docker exec -it <container name> /bin/sh
```

```
docker exec -it <container name> /bin/bash
```
## Stop
```
docker stop <container name>
```
## Remove
```
docker rm <container name>
```


# Files
## Dockerfile
Basic example:
```
# 1. BASE IMAGE
# Start from a base image. This determines the operating system environment.
FROM ubuntu:22.04

# 2. METADATA
# Provide metadata about who maintains the image.
# LABEL <key>="<value>"
# Adds **metadata** to the image, suchliche as the maintainer, version, or description.
LABEL maintainer="your-name@example.com"

# 3. ENVIRONMENT VARIABLES
# Set environment variables that will be available inside the container.
ENV APP_HOME=/app
ENV PORT=8080

# 4. WORKING DIRECTORY
# Set the default working directory for all subsequent commands.
WORKDIR $APP_HOME

# 5. COPY FILES
# Copy local files/directories from the build context (your host machine) into the image.
# We copy a hypothetical application code directory.
COPY . $APP_HOME

# 6. RUN COMMANDS
# Execute commands during the image build process (e.g., install software, update packages).
# Commands are typically chained with '&&' to minimize the number of layers.
RUN apt-get update && \
    apt-get install -y python3 python3-pip && \
    pip3 install --no-cache-dir -r requirements.txt && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 7. EXPOSE PORTS
# Inform Docker that the container listens on the specified network ports at runtime.
# This is documentation, not a firewall rule!
EXPOSE $PORT

# 8. ENTRYPOINT
# Defines the executable that will always run when the container starts.
# CMD arguments are appended to ENTRYPOINT.
# ENTRYPOINT is typically used for a script that handles initialization.
ENTRYPOINT ["python3"]

# 9. COMMAND (CMD)
# Provides default arguments for the ENTRYPOINT or an executable if ENTRYPOINT is not set.
# This is the command executed when running the container without specifying a command.
CMD ["app.py"]
```


## docker-compose.yaml

# Advanced

## Image
```
docker images
```
## Compose

| **Befehl**                 | **Funktion**                                                                                                                                                                                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`docker compose up`**    | **Der wichtigste Befehl.** Erstellt und startet alle Dienste, Netzwerke und Volumes aus der Konfigurationsdatei. Fügt standardmäßig das Flag `-d` (detached mode/Hintergrund) hinzu, wenn es nicht im Vordergrund laufen soll. |
| **`docker compose down`**  | **Stoppt und entfernt** alle Container, Netzwerke und Volumes (wenn sie nicht explizit als extern definiert wurden), die von $\text{up}$ erstellt wurden.                                                                      |
| **`docker compose build`** | Erzwingt den Neu-Build von Images, die über den `build`-Parameter definiert wurden.                                                                                                                                            |
| **`docker compose ps`**    | Zeigt den Status aller von Compose verwalteten Dienste.                                                                                                                                                                        |
| **`docker compose logs`**  | Zeigt die aggregierten Logs aller Dienste an.                                                                                                                                                                                  |

## container

### prune
Remove all stopped containers:
```
docker container prune
```
