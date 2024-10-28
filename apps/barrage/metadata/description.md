
# JSON
```json
{
  "services": [
    {
      "name": "barrage",
      "image": "maulik9898/barrage:0.3.0",
      "isMain": true,
      "internalPort": 3000,
      "environment": {
        "NEXTAUTH_SECRET": "${NEXTAUTH_SECRET}",
        "DELUGE_URL": "${DELUGE_URL}",
        "DELUGE_PASSWORD": "${DELUGE_PASSWORD}",
        "BARRAGE_PASSWORD": "${BARRAGE_PASSWORD}"
      }
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  barrage:
    image: maulik9898/barrage:0.3.0
    container_name: barrage
    environment:
    - NEXTAUTH_SECRET=${NEXTAUTH_SECRET}
    - DELUGE_URL=${DELUGE_URL}
    - DELUGE_PASSWORD=${DELUGE_PASSWORD}
    - BARRAGE_PASSWORD=${BARRAGE_PASSWORD}
    ports:
    - ${APP_PORT}:3000
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.barrage-web-redirect.redirectscheme.scheme: https
      traefik.http.services.barrage.loadbalancer.server.port: 3000
      traefik.http.routers.barrage-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.barrage-insecure.entrypoints: web
      traefik.http.routers.barrage-insecure.service: barrage
      traefik.http.routers.barrage-insecure.middlewares: barrage-web-redirect
      traefik.http.routers.barrage.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.barrage.entrypoints: websecure
      traefik.http.routers.barrage.service: barrage
      traefik.http.routers.barrage.tls.certresolver: myresolver
      traefik.http.routers.barrage-local-insecure.rule: Host(`barrage.${LOCAL_DOMAIN}`)
      traefik.http.routers.barrage-local-insecure.entrypoints: web
      traefik.http.routers.barrage-local-insecure.service: barrage
      traefik.http.routers.barrage-local-insecure.middlewares: barrage-web-redirect
      traefik.http.routers.barrage-local.rule: Host(`barrage.${LOCAL_DOMAIN}`)
      traefik.http.routers.barrage-local.entrypoints: websecure
      traefik.http.routers.barrage-local.service: barrage
      traefik.http.routers.barrage-local.tls: true
      runtipi.managed: true
 
```
