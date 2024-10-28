
# JSON
```json
{
  "services": [
    {
      "name": "olivetin",
      "image": "jamesread/olivetin:latest",
      "isMain": true,
      "internalPort": 1337,
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data",
          "containerPath": "/config"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '3.9'
services:
  olivetin:
    container_name: olivetin
    image: jamesread/olivetin:latest
    restart: unless-stopped
    volumes:
    - ${APP_DATA_DIR}/data:/config
    ports:
    - ${APP_PORT}:1337
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.olivetin-web-redirect.redirectscheme.scheme: https
      traefik.http.services.olivetin.loadbalancer.server.port: 1337
      traefik.http.routers.olivetin-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.olivetin-insecure.entrypoints: web
      traefik.http.routers.olivetin-insecure.service: olivetin
      traefik.http.routers.olivetin-insecure.middlewares: olivetin-web-redirect
      traefik.http.routers.olivetin.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.olivetin.entrypoints: websecure
      traefik.http.routers.olivetin.service: olivetin
      traefik.http.routers.olivetin.tls.certresolver: myresolver
      traefik.http.routers.olivetin-local-insecure.rule: Host(`olivetin.${LOCAL_DOMAIN}`)
      traefik.http.routers.olivetin-local-insecure.entrypoints: web
      traefik.http.routers.olivetin-local-insecure.service: olivetin
      traefik.http.routers.olivetin-local-insecure.middlewares: olivetin-web-redirect
      traefik.http.routers.olivetin-local.rule: Host(`olivetin.${LOCAL_DOMAIN}`)
      traefik.http.routers.olivetin-local.entrypoints: websecure
      traefik.http.routers.olivetin-local.service: olivetin
      traefik.http.routers.olivetin-local.tls: true
      runtipi.managed: true
 
```
