
# JSON
```json
{
  "services": [
    {
      "name": "searxng",
      "image": "searxng/searxng:latest",
      "isMain": true,
      "internalPort": 8080,
      "environment": {
        "BIND_ADDRESS": "0.0.0.0:8080",
        "BASE_URL": "${APP_PROTOCOL:-http}://${APP_DOMAIN}/",
        "SEARXNG_SECRET": "${SEARXNG_SECRET_KEY}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data",
          "containerPath": "/etc/searxng"
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
  searxng:
    container_name: searxng
    image: searxng/searxng:latest
    restart: unless-stopped
    networks:
    - tipi_main_network
    volumes:
    - ${APP_DATA_DIR}/data:/etc/searxng
    ports:
    - ${APP_PORT}:8080
    environment:
    - BIND_ADDRESS=0.0.0.0:8080
    - BASE_URL=${APP_PROTOCOL:-http}://${APP_DOMAIN}/
    - SEARXNG_SECRET=${SEARXNG_SECRET_KEY}
    labels:
      traefik.enable: true
      traefik.http.middlewares.searxng-web-redirect.redirectscheme.scheme: https
      traefik.http.services.searxng.loadbalancer.server.port: 8080
      traefik.http.routers.searxng-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.searxng-insecure.entrypoints: web
      traefik.http.routers.searxng-insecure.service: searxng
      traefik.http.routers.searxng-insecure.middlewares: searxng-web-redirect
      traefik.http.routers.searxng.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.searxng.entrypoints: websecure
      traefik.http.routers.searxng.service: searxng
      traefik.http.routers.searxng.tls.certresolver: myresolver
      traefik.http.routers.searxng-local-insecure.rule: Host(`searxng.${LOCAL_DOMAIN}`)
      traefik.http.routers.searxng-local-insecure.entrypoints: web
      traefik.http.routers.searxng-local-insecure.service: searxng
      traefik.http.routers.searxng-local-insecure.middlewares: searxng-web-redirect
      traefik.http.routers.searxng-local.rule: Host(`searxng.${LOCAL_DOMAIN}`)
      traefik.http.routers.searxng-local.entrypoints: websecure
      traefik.http.routers.searxng-local.service: searxng
      traefik.http.routers.searxng-local.tls: true
      runtipi.managed: true
 
```
