
# JSON
```json
{
  "services": [
    {
      "name": "podfetch",
      "image": "samuel19982/podfetch:latest",
      "isMain": true,
      "internalPort": 8000,
      "environment": {
        "BASIC_AUTH": "true",
        "USERNAME": "${PODFETCH_USERNAME}",
        "PASSWORD": "${PODFETCH_PASSWORD}",
        "POLLING_INTERVAL": "60",
        "SERVER_URL": "${APP_PROTOCOL:-http}://${APP_DOMAIN}",
        "PODINDEX_API_KEY": "\"${PODFETCH_PODINDEX_API_KEY}\"",
        "PODINDEX_API_SECRET": "\"${PODFETCH_PODINDEX_API_SECRET}\""
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/podfetch-db",
          "containerPath": "/app/db"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/data/podcasts",
          "containerPath": "/app/podcasts"
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
  podfetch:
    container_name: podfetch
    image: samuel19982/podfetch:latest
    ports:
    - ${APP_PORT}:8000
    volumes:
    - ${APP_DATA_DIR}/data/podfetch-db:/app/db
    - ${ROOT_FOLDER_HOST}/media/data/podcasts:/app/podcasts
    environment:
    - BASIC_AUTH=true
    - USERNAME=${PODFETCH_USERNAME}
    - PASSWORD=${PODFETCH_PASSWORD}
    - POLLING_INTERVAL=60
    - SERVER_URL=${APP_PROTOCOL:-http}://${APP_DOMAIN}
    - PODINDEX_API_KEY="${PODFETCH_PODINDEX_API_KEY}"
    - PODINDEX_API_SECRET="${PODFETCH_PODINDEX_API_SECRET}"
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.podfetch-web-redirect.redirectscheme.scheme: https
      traefik.http.services.podfetch.loadbalancer.server.port: 8000
      traefik.http.routers.podfetch-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.podfetch-insecure.entrypoints: web
      traefik.http.routers.podfetch-insecure.service: podfetch
      traefik.http.routers.podfetch-insecure.middlewares: podfetch-web-redirect
      traefik.http.routers.podfetch.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.podfetch.entrypoints: websecure
      traefik.http.routers.podfetch.service: podfetch
      traefik.http.routers.podfetch.tls.certresolver: myresolver
      traefik.http.routers.podfetch-local-insecure.rule: Host(`podfetch.${LOCAL_DOMAIN}`)
      traefik.http.routers.podfetch-local-insecure.entrypoints: web
      traefik.http.routers.podfetch-local-insecure.service: podfetch
      traefik.http.routers.podfetch-local-insecure.middlewares: podfetch-web-redirect
      traefik.http.routers.podfetch-local.rule: Host(`podfetch.${LOCAL_DOMAIN}`)
      traefik.http.routers.podfetch-local.entrypoints: websecure
      traefik.http.routers.podfetch-local.service: podfetch
      traefik.http.routers.podfetch-local.tls: true
      runtipi.managed: true
 
```
