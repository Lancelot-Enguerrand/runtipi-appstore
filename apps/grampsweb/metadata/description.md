
# JSON
```json
{
  "services": [
    {
      "name": "grampsweb",
      "image": "ghcr.io/gramps-project/grampsweb:v24.8.0",
      "isMain": true,
      "internalPort": 5000,
      "environment": {
        "GRAMPSWEB_TREE": "${GRAMPSWEB_TREE}",
        "GUNICORN_NUM_WORKERS": 1
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/users",
          "containerPath": "/app/users"
        },
        {
          "hostPath": "${APP_DATA_DIR}/index",
          "containerPath": "/app/indexdir"
        },
        {
          "hostPath": "${APP_DATA_DIR}/thumb_cache",
          "containerPath": "/app/thumbnail_cache"
        },
        {
          "hostPath": "${APP_DATA_DIR}/cache",
          "containerPath": "/app/cache"
        },
        {
          "hostPath": "${APP_DATA_DIR}/secret",
          "containerPath": "/app/secret"
        },
        {
          "hostPath": "${APP_DATA_DIR}/db",
          "containerPath": "/root/.gramps/grampsdb"
        },
        {
          "hostPath": "${APP_DATA_DIR}/media",
          "containerPath": "/app/media"
        },
        {
          "hostPath": "${APP_DATA_DIR}/tmp",
          "containerPath": "/tmp"
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
  grampsweb:
    image: ghcr.io/gramps-project/grampsweb:v24.8.0
    container_name: grampsweb
    restart: unless-stopped
    ports:
    - ${APP_PORT}:5000
    environment:
      GRAMPSWEB_TREE: ${GRAMPSWEB_TREE}
      GUNICORN_NUM_WORKERS: 1
    volumes:
    - ${APP_DATA_DIR}/users:/app/users
    - ${APP_DATA_DIR}/index:/app/indexdir
    - ${APP_DATA_DIR}/thumb_cache:/app/thumbnail_cache
    - ${APP_DATA_DIR}/cache:/app/cache
    - ${APP_DATA_DIR}/secret:/app/secret
    - ${APP_DATA_DIR}/db:/root/.gramps/grampsdb
    - ${APP_DATA_DIR}/media:/app/media
    - ${APP_DATA_DIR}/tmp:/tmp
    networks:
    - tipi_main_network
    labels:
      runtipi.managed: true
      traefik.enable: true
      traefik.http.middlewares.grampsweb-web-redirect.redirectscheme.scheme: https
      traefik.http.services.grampsweb.loadbalancer.server.port: 80
      traefik.http.routers.grampsweb-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.grampsweb-insecure.entrypoints: web
      traefik.http.routers.grampsweb-insecure.service: grampsweb
      traefik.http.routers.grampsweb-insecure.middlewares: grampsweb-web-redirect
      traefik.http.routers.grampsweb.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.grampsweb.entrypoints: websecure
      traefik.http.routers.grampsweb.service: grampsweb
      traefik.http.routers.grampsweb.tls.certresolver: myresolver
      traefik.http.routers.grampsweb-local-insecure.rule: Host(`grampsweb.${LOCAL_DOMAIN}`)
      traefik.http.routers.grampsweb-local-insecure.entrypoints: web
      traefik.http.routers.grampsweb-local-insecure.service: grampsweb
      traefik.http.routers.grampsweb-local-insecure.middlewares: grampsweb-web-redirect
      traefik.http.routers.grampsweb-local.rule: Host(`grampsweb.${LOCAL_DOMAIN}`)
      traefik.http.routers.grampsweb-local.entrypoints: websecure
      traefik.http.routers.grampsweb-local.service: grampsweb
      traefik.http.routers.grampsweb-local.tls: true
 
```
