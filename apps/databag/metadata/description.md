
# JSON
```json
{
  "services": [
    {
      "name": "databag",
      "image": "balzack/databag:latest",
      "isMain": true,
      "internalPort": 7000,
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/databag",
          "containerPath": "/var/lib/databag"
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
  databag:
    container_name: databag
    image: balzack/databag:latest
    ports:
    - ${APP_PORT}:7000
    volumes:
    - ${APP_DATA_DIR}/data/databag:/var/lib/databag
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.databag-web-redirect.redirectscheme.scheme: https
      traefik.http.services.databag.loadbalancer.server.port: 7000
      traefik.http.routers.databag-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.databag-insecure.entrypoints: web
      traefik.http.routers.databag-insecure.service: databag
      traefik.http.routers.databag-insecure.middlewares: databag-web-redirect
      traefik.http.routers.databag.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.databag.entrypoints: websecure
      traefik.http.routers.databag.service: databag
      traefik.http.routers.databag.tls.certresolver: myresolver
      traefik.http.routers.databag-local-insecure.rule: Host(`databag.${LOCAL_DOMAIN}`)
      traefik.http.routers.databag-local-insecure.entrypoints: web
      traefik.http.routers.databag-local-insecure.service: databag
      traefik.http.routers.databag-local-insecure.middlewares: databag-web-redirect
      traefik.http.routers.databag-local.rule: Host(`databag.${LOCAL_DOMAIN}`)
      traefik.http.routers.databag-local.entrypoints: websecure
      traefik.http.routers.databag-local.service: databag
      traefik.http.routers.databag-local.tls: true
      runtipi.managed: true
 
```
