
# JSON
```json
{
  "services": [
    {
      "name": "libreddit",
      "image": "libreddit/libreddit:latest",
      "isMain": true,
      "internalPort": 8080
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  libreddit:
    container_name: libreddit
    image: libreddit/libreddit:latest
    dns:
    - ${DNS_IP}
    ports:
    - ${APP_PORT}:8080
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.libreddit-web-redirect.redirectscheme.scheme: https
      traefik.http.services.libreddit.loadbalancer.server.port: 8080
      traefik.http.routers.libreddit-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.libreddit-insecure.entrypoints: web
      traefik.http.routers.libreddit-insecure.service: libreddit
      traefik.http.routers.libreddit-insecure.middlewares: libreddit-web-redirect
      traefik.http.routers.libreddit.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.libreddit.entrypoints: websecure
      traefik.http.routers.libreddit.service: libreddit
      traefik.http.routers.libreddit.tls.certresolver: myresolver
      traefik.http.routers.libreddit-local-insecure.rule: Host(`libreddit.${LOCAL_DOMAIN}`)
      traefik.http.routers.libreddit-local-insecure.entrypoints: web
      traefik.http.routers.libreddit-local-insecure.service: libreddit
      traefik.http.routers.libreddit-local-insecure.middlewares: libreddit-web-redirect
      traefik.http.routers.libreddit-local.rule: Host(`libreddit.${LOCAL_DOMAIN}`)
      traefik.http.routers.libreddit-local.entrypoints: websecure
      traefik.http.routers.libreddit-local.service: libreddit
      traefik.http.routers.libreddit-local.tls: true
      runtipi.managed: true
 
```
