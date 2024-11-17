
# JSON
```json
{
  "services": [
    {
      "name": "minecraft-server",
      "image": "itzg/minecraft-server:latest",
      "isMain": true,
      "internalPort": 25565,
      "environment": {
        "EULA": "TRUE",
        "VERSION": "${MC_VERSION:-LATEST}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/minecraft-data",
          "containerPath": "/data"
        }
      ],
      "tty": true,
      "stdinOpen": true
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  minecraft-server:
    container_name: minecraft-server
    image: itzg/minecraft-server:latest
    ports:
    - ${APP_PORT}:25565
    environment:
      EULA: 'TRUE'
      VERSION: ${MC_VERSION:-LATEST}
    tty: true
    stdin_open: true
    restart: unless-stopped
    volumes:
    - ${APP_DATA_DIR}/data/minecraft-data:/data
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.minecraft-server-web-redirect.redirectscheme.scheme: https
      traefik.http.services.minecraft-server.loadbalancer.server.port: 25565
      traefik.http.routers.minecraft-server-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.minecraft-server-insecure.entrypoints: web
      traefik.http.routers.minecraft-server-insecure.service: minecraft-server
      traefik.http.routers.minecraft-server-insecure.middlewares: minecraft-server-web-redirect
      traefik.http.routers.minecraft-server.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.minecraft-server.entrypoints: websecure
      traefik.http.routers.minecraft-server.service: minecraft-server
      traefik.http.routers.minecraft-server.tls.certresolver: myresolver
      traefik.http.routers.minecraft-server-local-insecure.rule: Host(`minecraft-server.${LOCAL_DOMAIN}`)
      traefik.http.routers.minecraft-server-local-insecure.entrypoints: web
      traefik.http.routers.minecraft-server-local-insecure.service: minecraft-server
      traefik.http.routers.minecraft-server-local-insecure.middlewares: minecraft-server-web-redirect
      traefik.http.routers.minecraft-server-local.rule: Host(`minecraft-server.${LOCAL_DOMAIN}`)
      traefik.http.routers.minecraft-server-local.entrypoints: websecure
      traefik.http.routers.minecraft-server-local.service: minecraft-server
      traefik.http.routers.minecraft-server-local.tls: true
      runtipi.managed: true
 
```
