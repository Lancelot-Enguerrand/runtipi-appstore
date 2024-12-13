
# JSON
```json
{
  "services": [
    {
      "name": "booksonic",
      "image": "lscr.io/linuxserver/booksonic-air:2201.1.0",
      "isMain": true,
      "internalPort": 4040,
      "environment": {
        "PUID": "1000",
        "PGID": "1000",
        "TZ": "${TZ}",
        "CONTEXT_PATH": "{APP_PROTOCOL:-http}://${APP_DOMAIN}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/config",
          "containerPath": "/config"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/data/books/spoken",
          "containerPath": "/audiobooks"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/data/podcasts",
          "containerPath": "/podcasts"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  booksonic:
    image: lscr.io/linuxserver/booksonic-air:2201.1.0
    container_name: booksonic
    environment:
    - PUID=1000
    - PGID=1000
    - TZ=${TZ}
    - CONTEXT_PATH={APP_PROTOCOL:-http}://${APP_DOMAIN}
    volumes:
    - ${APP_DATA_DIR}/config:/config
    - ${ROOT_FOLDER_HOST}/media/data/books/spoken:/audiobooks
    - ${ROOT_FOLDER_HOST}/media/data/podcasts:/podcasts
    ports:
    - ${APP_PORT}:4040
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.booksonic-web-redirect.redirectscheme.scheme: https
      traefik.http.services.booksonic.loadbalancer.server.port: 4040
      traefik.http.routers.booksonic-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.booksonic-insecure.entrypoints: web
      traefik.http.routers.booksonic-insecure.service: booksonic
      traefik.http.routers.booksonic-insecure.middlewares: booksonic-web-redirect
      traefik.http.routers.booksonic.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.booksonic.entrypoints: websecure
      traefik.http.routers.booksonic.service: booksonic
      traefik.http.routers.booksonic.tls.certresolver: myresolver
      traefik.http.routers.booksonic-local-insecure.rule: Host(`booksonic.${LOCAL_DOMAIN}`)
      traefik.http.routers.booksonic-local-insecure.entrypoints: web
      traefik.http.routers.booksonic-local-insecure.service: booksonic
      traefik.http.routers.booksonic-local-insecure.middlewares: booksonic-web-redirect
      traefik.http.routers.booksonic-local.rule: Host(`booksonic.${LOCAL_DOMAIN}`)
      traefik.http.routers.booksonic-local.entrypoints: websecure
      traefik.http.routers.booksonic-local.service: booksonic
      traefik.http.routers.booksonic-local.tls: true
      runtipi.managed: true
 
```
