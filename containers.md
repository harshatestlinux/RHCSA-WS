# Container Management (Podman)

## Basic Container Operations
- `podman run [options] image [command]` - Run a container
  - `-d` - Run in background (detached)
  - `-it` - Interactive with TTY
  - `-p host:container` - Port mapping
  - `-v host:container` - Volume mounting
  - `--name mycontainer` - Assign name to container
- `podman ps` - List running containers
- `podman ps -a` - List all containers
- `podman stop container` - Stop a container
- `podman start container` - Start a stopped container
- `podman restart container` - Restart a container
- `podman rm container` - Remove a container
- `podman rm -f container` - Force remove running container

## Image Management
- `podman images` - List local images
- `podman pull image:tag` - Download an image
- `podman rmi image` - Remove an image
- `podman search term` - Search for images
- `podman inspect image` - Show detailed image information
- `podman history image` - Show image layer history

## Container Information
- `podman logs container` - Show container logs
- `podman exec -it container /bin/bash` - Execute command in running container
- `podman inspect container` - Show detailed container information
- `podman top container` - Show running processes in container
- `podman stats` - Show container resource usage

## Persistent Storage
- `podman volume create myvolume` - Create a volume
- `podman volume ls` - List volumes
- `podman volume inspect myvolume` - Show volume details
- `podman volume rm myvolume` - Remove a volume
- `podman run -v myvolume:/data image` - Mount volume in container

## Container as Services
- `podman generate systemd container > /etc/systemd/system/container.service` - Generate systemd unit
- `systemctl enable container.service` - Enable container service
- `systemctl start container.service` - Start container service
- `loginctl enable-linger username` - Enable user services at boot

## Rootless Containers
- `podman run --user 1000:1000 image` - Run as specific user
- `podman unshare` - Enter user namespace
- `podman system migrate` - Migrate containers to new storage

## Container Networking
- `podman network ls` - List networks
- `podman network create mynetwork` - Create custom network
- `podman run --network mynetwork image` - Use custom network
- `podman port container` - Show port mappings

## Troubleshooting
- `podman system info` - Show system information
- `podman system events` - Show system events
- `podman system df` - Show disk usage
- `podman system prune` - Remove unused data
