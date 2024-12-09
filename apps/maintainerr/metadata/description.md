
# JSON
```json
{
  "services": [
    {
      "name": "maintainerr",
      "image": "ghcr.io/jorenn92/maintainerr:2.2.1",
      "isMain": true,
      "internalPort": 6246,
      "environment": {
        "TZ": "${TZ}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/config",
          "containerPath": "/opt/data"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '3'
services:
  maintainerr:
    image: ghcr.io/jorenn92/maintainerr:2.2.1
    container_name: maintainerr
    volumes:
    - ${APP_DATA_DIR}/data/config:/opt/data
    environment:
    - TZ=${TZ}
    ports:
    - ${APP_PORT}:6246
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.maintainerr-web-redirect.redirectscheme.scheme: https
      traefik.http.services.maintainerr.loadbalancer.server.port: 6246
      traefik.http.routers.maintainerr-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.maintainerr-insecure.entrypoints: web
      traefik.http.routers.maintainerr-insecure.service: maintainerr
      traefik.http.routers.maintainerr-insecure.middlewares: maintainerr-web-redirect
      traefik.http.routers.maintainerr.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.maintainerr.entrypoints: websecure
      traefik.http.routers.maintainerr.service: maintainerr
      traefik.http.routers.maintainerr.tls.certresolver: myresolver
      traefik.http.routers.maintainerr-local-insecure.rule: Host(`maintainerr.${LOCAL_DOMAIN}`)
      traefik.http.routers.maintainerr-local-insecure.entrypoints: web
      traefik.http.routers.maintainerr-local-insecure.service: maintainerr
      traefik.http.routers.maintainerr-local-insecure.middlewares: maintainerr-web-redirect
      traefik.http.routers.maintainerr-local.rule: Host(`maintainerr.${LOCAL_DOMAIN}`)
      traefik.http.routers.maintainerr-local.entrypoints: websecure
      traefik.http.routers.maintainerr-local.service: maintainerr
      traefik.http.routers.maintainerr-local.tls: true
      runtipi.managed: true
 
```
