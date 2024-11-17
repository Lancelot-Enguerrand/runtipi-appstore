
# JSON
```json
{
  "services": [
    {
      "name": "deluge",
      "image": "lscr.io/linuxserver/deluge:2.1.1",
      "isMain": true,
      "internalPort": 8112,
      "addPorts": [
        {
          "hostPort": 6881,
          "containerPort": 6881,
          "udp": true
        }
      ],
      "environment": {
        "PUID": "1000",
        "PGID": "1000",
        "TZ": "${TZ}",
        "DELUGE_LOGLEVEL": "error"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/deluge/config",
          "containerPath": "/config"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/torrents",
          "containerPath": "/media/torrents"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '2.1'
services:
  deluge:
    image: lscr.io/linuxserver/deluge:2.1.1
    container_name: deluge
    environment:
    - PUID=1000
    - PGID=1000
    - TZ=${TZ}
    - DELUGE_LOGLEVEL=error
    volumes:
    - ${APP_DATA_DIR}/data/deluge/config:/config
    - ${ROOT_FOLDER_HOST}/media/torrents:/media/torrents
    ports:
    - ${APP_PORT}:8112
    - 6881:6881
    - 6881:6881/udp
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.deluge-web-redirect.redirectscheme.scheme: https
      traefik.http.services.deluge.loadbalancer.server.port: 8112
      traefik.http.routers.deluge-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.deluge-insecure.entrypoints: web
      traefik.http.routers.deluge-insecure.service: deluge
      traefik.http.routers.deluge-insecure.middlewares: deluge-web-redirect
      traefik.http.routers.deluge.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.deluge.entrypoints: websecure
      traefik.http.routers.deluge.service: deluge
      traefik.http.routers.deluge.tls.certresolver: myresolver
      traefik.http.routers.deluge-local-insecure.rule: Host(`deluge.${LOCAL_DOMAIN}`)
      traefik.http.routers.deluge-local-insecure.entrypoints: web
      traefik.http.routers.deluge-local-insecure.service: deluge
      traefik.http.routers.deluge-local-insecure.middlewares: deluge-web-redirect
      traefik.http.routers.deluge-local.rule: Host(`deluge.${LOCAL_DOMAIN}`)
      traefik.http.routers.deluge-local.entrypoints: websecure
      traefik.http.routers.deluge-local.service: deluge
      traefik.http.routers.deluge-local.tls: true
      runtipi.managed: true
 
```
