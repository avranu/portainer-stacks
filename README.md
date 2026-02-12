# portainer-stacks
docker-compose.yml files to be used with Portainer to deploy apps. This is primarily used within a homelab.

## Usage
These containers typically require directories to be created on the host machine. On TrueNas Scale, I create these under /mnt/{pool}/containers/{app}, and I set the ownership to apps:apps.

```
mkdir -p /mnt/trunk/containers/gitlab
chown apps:apps /mnt/trunk/containers/gitlab
```

Then, open portainer's web ui, and click on the "Stacks" tab. Copy the docker-compose.yml into the web editor, click on "Environment Variables", click on "Advanced", and copy stack.env into it. Modify the values as needed.

## Security

Make sure you change all keys and passwords. Examples are included, but they are published in a public repo, so are absolutely not secure.

## Ports

In order to avoid port collisions, I use uncommon ports for all of these apps. The APP_PORT_PREFIX variable is used to set the first two digits of the port, and then the app's default port is used for the last three digits. For example, if an app's default port is 80, and the APP_PORT_PREFIX is 30, then the app will be exposed on port 30080. This makes it easy to expose a series of apps without worrying about port collisions.