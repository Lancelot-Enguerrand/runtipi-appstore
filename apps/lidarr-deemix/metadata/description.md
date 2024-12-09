
# JSON
```json
{
  "services": [
    {
      "name": "lidarr-deemix",
      "image": "youegraillot/lidarr-on-steroids:1.5.2",
      "isMain": true,
      "internalPort": 8686,
      "addPorts": [
        {
          "hostPort": 8187,
          "containerPort": 6595
        }
      ],
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/config",
          "containerPath": "/config"
        },
        {
          "hostPath": "${APP_DATA_DIR}/data/config-deemix",
          "containerPath": "/config_deemix"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/downloads/deemix",
          "containerPath": "/downloads"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/data/music",
          "containerPath": "/music"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/usenet/completed/",
          "containerPath": "/downloads/completed"
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
  lidarr-deemix:
    image: youegraillot/lidarr-on-steroids:1.5.2
    container_name: lidarr-deemix
    volumes:
    - ${APP_DATA_DIR}/data/config:/config
    - ${APP_DATA_DIR}/data/config-deemix:/config_deemix
    - ${ROOT_FOLDER_HOST}/media/downloads/deemix:/downloads
    - ${ROOT_FOLDER_HOST}/media/data/music:/music
    - ${ROOT_FOLDER_HOST}/media/usenet/completed/:/downloads/completed
    - ${ROOT_FOLDER_HOST}/media:/media
    ports:
    - ${APP_PORT}:8686
    - 8187:6595
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.lidarr-deemix-web-redirect.redirectscheme.scheme: https
      traefik.http.services.lidarr-deemix.loadbalancer.server.port: 8686
      traefik.http.routers.lidarr-deemix-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.lidarr-deemix-insecure.entrypoints: web
      traefik.http.routers.lidarr-deemix-insecure.service: lidarr-deemix
      traefik.http.routers.lidarr-deemix-insecure.middlewares: lidarr-deemix-web-redirect
      traefik.http.routers.lidarr-deemix.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.lidarr-deemix.entrypoints: websecure
      traefik.http.routers.lidarr-deemix.service: lidarr-deemix
      traefik.http.routers.lidarr-deemix.tls.certresolver: myresolver
      traefik.http.routers.lidarr-deemix-local-insecure.rule: Host(`lidarr-deemix.${LOCAL_DOMAIN}`)
      traefik.http.routers.lidarr-deemix-local-insecure.entrypoints: web
      traefik.http.routers.lidarr-deemix-local-insecure.service: lidarr-deemix
      traefik.http.routers.lidarr-deemix-local-insecure.middlewares: lidarr-deemix-web-redirect
      traefik.http.routers.lidarr-deemix-local.rule: Host(`lidarr-deemix.${LOCAL_DOMAIN}`)
      traefik.http.routers.lidarr-deemix-local.entrypoints: websecure
      traefik.http.routers.lidarr-deemix-local.service: lidarr-deemix
      traefik.http.routers.lidarr-deemix-local.tls: true
      runtipi.managed: true
 
```
