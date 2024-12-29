
# JSON
```json
{
  "services": [
    {
      "name": "bazarr",
      "image": "lscr.io/linuxserver/bazarr:1.5.0",
      "isMain": true,
      "internalPort": 6767,
      "environment": {
        "PUID": "1000",
        "PGID": "1000",
        "TZ": "${TZ}"
      },
      "volumes": [
        {
          "hostPath": "/etc/localtime",
          "containerPath": "/etc/localtime",
          "readOnly": true
        },
        {
          "hostPath": "${APP_DATA_DIR}/data",
          "containerPath": "/config"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media",
          "containerPath": "/media"
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
  bazarr:
    image: lscr.io/linuxserver/bazarr:1.5.0
    container_name: bazarr
    environment:
    - PUID=1000
    - PGID=1000
    - TZ=${TZ}
    dns:
    - ${DNS_IP}
    volumes:
    - /etc/localtime:/etc/localtime:ro
    - ${APP_DATA_DIR}/data:/config
    - ${ROOT_FOLDER_HOST}/media:/media
    ports:
    - ${APP_PORT}:6767
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.bazarr-web-redirect.redirectscheme.scheme: https
      traefik.http.services.bazarr.loadbalancer.server.port: 6767
      traefik.http.routers.bazarr-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.bazarr-insecure.entrypoints: web
      traefik.http.routers.bazarr-insecure.service: bazarr
      traefik.http.routers.bazarr-insecure.middlewares: bazarr-web-redirect
      traefik.http.routers.bazarr.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.bazarr.entrypoints: websecure
      traefik.http.routers.bazarr.service: bazarr
      traefik.http.routers.bazarr.tls.certresolver: myresolver
      traefik.http.routers.bazarr-local-insecure.rule: Host(`bazarr.${LOCAL_DOMAIN}`)
      traefik.http.routers.bazarr-local-insecure.entrypoints: web
      traefik.http.routers.bazarr-local-insecure.service: bazarr
      traefik.http.routers.bazarr-local-insecure.middlewares: bazarr-web-redirect
      traefik.http.routers.bazarr-local.rule: Host(`bazarr.${LOCAL_DOMAIN}`)
      traefik.http.routers.bazarr-local.entrypoints: websecure
      traefik.http.routers.bazarr-local.service: bazarr
      traefik.http.routers.bazarr-local.tls: true
      runtipi.managed: true
 
```
