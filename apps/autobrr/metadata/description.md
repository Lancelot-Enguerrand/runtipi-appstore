# Checklist
## Dynamic compose for autobrr
This is a autobrr update for using dynamic compose.
##### Reaching the app :
- [ ] http://localip:port
- [ ] https://autobrr.tipi.local
##### In app tests :
- [ ] 📝 Register and log in
- [ ] 🖱 Basic interaction
- [ ] 🌆 Uploading data
- [ ] 🔄 Check data after restart
##### Volumes mapping :
- [ ] ${APP_DATA_DIR}/data/autobrr:/config
##### Specific instructions :
- [ ] 🌳 Environment
- [ ] 👤 User (1000:1000)

# New JSON
```json
{
  "$schema": "../dynamic-compose-schema.json",
  "services": [
    {
      "name": "autobrr",
      "image": "ghcr.io/autobrr/autobrr:v1.57.0",
      "isMain": true,
      "internalPort": 7474,
      "user": "1000:1000",
      "environment": {
        "TZ": "${TZ}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/autobrr",
          "containerPath": "/config"
        }
      ]
    }
  ]
} 
```
# Original YAML
```yaml
version: '3'
services:
  autobrr:
    container_name: autobrr
    image: ghcr.io/autobrr/autobrr:v1.57.0
    restart: unless-stopped
    ports:
    - ${APP_PORT}:7474
    volumes:
    - ${APP_DATA_DIR}/data/autobrr:/config
    user: 1000:1000
    environment:
    - TZ=${TZ}
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.autobrr-web-redirect.redirectscheme.scheme: https
      traefik.http.services.autobrr.loadbalancer.server.port: 7474
      traefik.http.routers.autobrr-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.autobrr-insecure.entrypoints: web
      traefik.http.routers.autobrr-insecure.service: autobrr
      traefik.http.routers.autobrr-insecure.middlewares: autobrr-web-redirect
      traefik.http.routers.autobrr.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.autobrr.entrypoints: websecure
      traefik.http.routers.autobrr.service: autobrr
      traefik.http.routers.autobrr.tls.certresolver: myresolver
      traefik.http.routers.autobrr-local-insecure.rule: Host(`autobrr.${LOCAL_DOMAIN}`)
      traefik.http.routers.autobrr-local-insecure.entrypoints: web
      traefik.http.routers.autobrr-local-insecure.service: autobrr
      traefik.http.routers.autobrr-local-insecure.middlewares: autobrr-web-redirect
      traefik.http.routers.autobrr-local.rule: Host(`autobrr.${LOCAL_DOMAIN}`)
      traefik.http.routers.autobrr-local.entrypoints: websecure
      traefik.http.routers.autobrr-local.service: autobrr
      traefik.http.routers.autobrr-local.tls: true
      runtipi.managed: true
 
```
