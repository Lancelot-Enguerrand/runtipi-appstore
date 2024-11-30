
# JSON
```json
{
  "services": [
    {
      "name": "sonarr",
      "image": "lscr.io/linuxserver/sonarr:4.0.11",
      "isMain": true,
      "internalPort": 8989,
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
  sonarr:
    image: lscr.io/linuxserver/sonarr:4.0.11
    container_name: sonarr
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
    - ${APP_PORT}:8989
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.sonarr-web-redirect.redirectscheme.scheme: https
      traefik.http.services.sonarr.loadbalancer.server.port: 8989
      traefik.http.routers.sonarr-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.sonarr-insecure.entrypoints: web
      traefik.http.routers.sonarr-insecure.service: sonarr
      traefik.http.routers.sonarr-insecure.middlewares: sonarr-web-redirect
      traefik.http.routers.sonarr.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.sonarr.entrypoints: websecure
      traefik.http.routers.sonarr.service: sonarr
      traefik.http.routers.sonarr.tls.certresolver: myresolver
      traefik.http.routers.sonarr-local-insecure.rule: Host(`sonarr.${LOCAL_DOMAIN}`)
      traefik.http.routers.sonarr-local-insecure.entrypoints: web
      traefik.http.routers.sonarr-local-insecure.service: sonarr
      traefik.http.routers.sonarr-local-insecure.middlewares: sonarr-web-redirect
      traefik.http.routers.sonarr-local.rule: Host(`sonarr.${LOCAL_DOMAIN}`)
      traefik.http.routers.sonarr-local.entrypoints: websecure
      traefik.http.routers.sonarr-local.service: sonarr
      traefik.http.routers.sonarr-local.tls: true
      runtipi.managed: true
 
```
