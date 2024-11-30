
# JSON
```json
{
  "services": [
    {
      "name": "it-tools",
      "image": "corentinth/it-tools:latest",
      "isMain": true,
      "internalPort": 80
    }
  ]
} 
```
# YAML
```yaml
version: '3'
services:
  it-tools:
    container_name: it-tools
    image: corentinth/it-tools:latest
    ports:
    - ${APP_PORT}:80
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.it-tools-web-redirect.redirectscheme.scheme: https
      traefik.http.services.it-tools.loadbalancer.server.port: 80
      traefik.http.routers.it-tools-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.it-tools-insecure.entrypoints: web
      traefik.http.routers.it-tools-insecure.service: it-tools
      traefik.http.routers.it-tools-insecure.middlewares: it-tools-web-redirect
      traefik.http.routers.it-tools.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.it-tools.entrypoints: websecure
      traefik.http.routers.it-tools.service: it-tools
      traefik.http.routers.it-tools.tls.certresolver: myresolver
      traefik.http.routers.it-tools-local-insecure.rule: Host(`it-tools.${LOCAL_DOMAIN}`)
      traefik.http.routers.it-tools-local-insecure.entrypoints: web
      traefik.http.routers.it-tools-local-insecure.service: it-tools
      traefik.http.routers.it-tools-local-insecure.middlewares: it-tools-web-redirect
      traefik.http.routers.it-tools-local.rule: Host(`it-tools.${LOCAL_DOMAIN}`)
      traefik.http.routers.it-tools-local.entrypoints: websecure
      traefik.http.routers.it-tools-local.service: it-tools
      traefik.http.routers.it-tools-local.tls: true
      runtipi.managed: true
 
```
