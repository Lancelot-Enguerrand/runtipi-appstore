
# JSON
```json
{
  "services": [
    {
      "name": "spacedrive",
      "image": "ghcr.io/spacedriveapp/spacedrive/server:0.4.2",
      "isMain": true,
      "internalPort": 8080,
      "environment": {
        "SD_AUTH": "${SD_AUTH_USER}:${SD_AUTH_PASSWORD}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/spacedrive",
          "containerPath": "/var/spacedrive"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '3.9'
services:
  spacedrive:
    container_name: spacedrive
    image: ghcr.io/spacedriveapp/spacedrive/server:0.4.2
    restart: unless-stopped
    environment:
    - SD_AUTH=${SD_AUTH_USER}:${SD_AUTH_PASSWORD}
    ports:
    - ${APP_PORT}:8080
    networks:
    - tipi_main_network
    volumes:
    - ${APP_DATA_DIR}/data/spacedrive:/var/spacedrive
    labels:
      traefik.enable: true
      traefik.http.middlewares.spacedrive-web-redirect.redirectscheme.scheme: https
      traefik.http.services.spacedrive.loadbalancer.server.port: 8080
      traefik.http.routers.spacedrive-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.spacedrive-insecure.entrypoints: web
      traefik.http.routers.spacedrive-insecure.service: spacedrive
      traefik.http.routers.spacedrive-insecure.middlewares: spacedrive-web-redirect
      traefik.http.routers.spacedrive.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.spacedrive.entrypoints: websecure
      traefik.http.routers.spacedrive.service: spacedrive
      traefik.http.routers.spacedrive.tls.certresolver: myresolver
      traefik.http.routers.spacedrive-local-insecure.rule: Host(`spacedrive.${LOCAL_DOMAIN}`)
      traefik.http.routers.spacedrive-local-insecure.entrypoints: web
      traefik.http.routers.spacedrive-local-insecure.service: spacedrive
      traefik.http.routers.spacedrive-local-insecure.middlewares: spacedrive-web-redirect
      traefik.http.routers.spacedrive-local.rule: Host(`spacedrive.${LOCAL_DOMAIN}`)
      traefik.http.routers.spacedrive-local.entrypoints: websecure
      traefik.http.routers.spacedrive-local.service: spacedrive
      traefik.http.routers.spacedrive-local.tls: true
      runtipi.managed: true
 
```
