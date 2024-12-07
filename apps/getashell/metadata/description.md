
# JSON
```json
{
  "services": [
    {
      "name": "getashell",
      "image": "ghcr.io/steveiliop56/getashell:v1.1.3",
      "isMain": true,
      "internalPort": 3000,
      "extraHosts": [
        "host.docker.internal:host-gateway"
      ],
      "environment": {
        "USERNAME": "${GETASHELL_USERNAME}",
        "PASSWORD": "${GETASHELL_PASSWORD}",
        "SECRET_KEY": "${GETASHELL_SECRET_KEY}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/",
          "containerPath": "/app/data"
        },
        {
          "hostPath": "/var/run/docker.sock",
          "containerPath": "/var/run/docker.sock",
          "readOnly": true
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
  getashell:
    image: ghcr.io/steveiliop56/getashell:v1.1.3
    container_name: getashell
    restart: unless-stopped
    volumes:
    - ${APP_DATA_DIR}/data/:/app/data
    - /var/run/docker.sock:/var/run/docker.sock:ro
    ports:
    - ${APP_PORT}:3000
    networks:
    - tipi_main_network
    environment:
    - USERNAME=${GETASHELL_USERNAME}
    - PASSWORD=${GETASHELL_PASSWORD}
    - SECRET_KEY=${GETASHELL_SECRET_KEY}
    extra_hosts:
    - host.docker.internal:host-gateway
    labels:
      traefik.enable: true
      traefik.http.middlewares.getashell-web-redirect.redirectscheme.scheme: https
      traefik.http.services.getashell.loadbalancer.server.port: 2368
      traefik.http.routers.getashell-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.getashell-insecure.entrypoints: web
      traefik.http.routers.getashell-insecure.service: getashell
      traefik.http.routers.getashell-insecure.middlewares: getashell-web-redirect
      traefik.http.routers.getashell.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.getashell.entrypoints: websecure
      traefik.http.routers.getashell.service: getashell
      traefik.http.routers.getashell.tls.certresolver: myresolver
      traefik.http.routers.getashell-local-insecure.rule: Host(`getashell.${LOCAL_DOMAIN}`)
      traefik.http.routers.getashell-local-insecure.entrypoints: web
      traefik.http.routers.getashell-local-insecure.service: getashell
      traefik.http.routers.getashell-local-insecure.middlewares: getashell-web-redirect
      traefik.http.routers.getashell-local.rule: Host(`getashell.${LOCAL_DOMAIN}`)
      traefik.http.routers.getashell-local.entrypoints: websecure
      traefik.http.routers.getashell-local.service: getashell
      traefik.http.routers.getashell-local.tls: true
      runtipi.managed: true
 
```
