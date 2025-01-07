# Checklist
## Dynamic compose for radarr
This is a radarr update for using dynamic compose.
##### Reaching the app :
- [ ] http://localip:port
- [ ] https://radarr.tipi.local
##### In app tests :
- [ ] 📝 Register and log in
- [ ] 🖱 Basic interaction
- [ ] 🌆 Uploading data
- [ ] 🔄 Check data after restart
##### Volumes mapping :
- [ ] /etc/localtime:/etc/localtime:ro
- [ ] ${APP_DATA_DIR}/data:/config
- [ ] ${ROOT_FOLDER_HOST}/media:/media
##### Specific instructions :
- [ ] 🌳 Environment
- 🏷 DNS (skipped)

# New JSON
```json
{
  "$schema": "../dynamic-compose-schema.json",
  "services": [
    {
      "name": "radarr",
      "image": "ghcr.io/linuxserver/radarr:5.17.2",
      "isMain": true,
      "internalPort": 7878,
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
# Original YAML
```yaml
version: '3.9'
services:
  radarr:
    image: ghcr.io/linuxserver/radarr:5.17.2
    container_name: radarr
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
    - ${APP_PORT}:7878
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.radarr-web-redirect.redirectscheme.scheme: https
      traefik.http.services.radarr.loadbalancer.server.port: 7878
      traefik.http.routers.radarr-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.radarr-insecure.entrypoints: web
      traefik.http.routers.radarr-insecure.service: radarr
      traefik.http.routers.radarr-insecure.middlewares: radarr-web-redirect
      traefik.http.routers.radarr.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.radarr.entrypoints: websecure
      traefik.http.routers.radarr.service: radarr
      traefik.http.routers.radarr.tls.certresolver: myresolver
      traefik.http.routers.radarr-local-insecure.rule: Host(`radarr.${LOCAL_DOMAIN}`)
      traefik.http.routers.radarr-local-insecure.entrypoints: web
      traefik.http.routers.radarr-local-insecure.service: radarr
      traefik.http.routers.radarr-local-insecure.middlewares: radarr-web-redirect
      traefik.http.routers.radarr-local.rule: Host(`radarr.${LOCAL_DOMAIN}`)
      traefik.http.routers.radarr-local.entrypoints: websecure
      traefik.http.routers.radarr-local.service: radarr
      traefik.http.routers.radarr-local.tls: true
      runtipi.managed: true
 
```
