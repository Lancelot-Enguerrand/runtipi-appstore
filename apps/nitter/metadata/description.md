
# JSON
```json
{
  "services": [
    {
      "name": "nitter",
      "image": "zedeus/nitter:latest",
      "isMain": true,
      "internalPort": 8080,
      "dependsOn": [
        "nitter-redis"
      ],
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/nitter.conf",
          "containerPath": "/src/nitter.conf",
          "readOnly": true
        }
      ],
      "healthCheck": {
        "interval": "1m",
        "timeout": "3s",
        "test": "wget --no-verbose --tries=1 --spider http://localhost:8080"
      }
    },
    {
      "name": "nitter-redis",
      "image": "redis:alpine",
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/redis",
          "containerPath": "/data"
        }
      ],
      "command": "redis-server --save 60 1 --loglevel warning"
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  nitter:
    image: zedeus/nitter:latest
    container_name: nitter
    networks:
    - tipi_main_network
    ports:
    - ${APP_PORT}:8080
    volumes:
    - ${APP_DATA_DIR}/data/nitter.conf:/src/nitter.conf:ro
    depends_on:
    - nitter-redis
    restart: unless-stopped
    healthcheck:
      test:
      - CMD
      - wget
      - --no-verbose
      - --tries=1
      - --spider
      - http://localhost:8080
      interval: 1m
      timeout: 3s
    labels:
      traefik.enable: true
      traefik.http.middlewares.nitter-web-redirect.redirectscheme.scheme: https
      traefik.http.services.nitter.loadbalancer.server.port: 8080
      traefik.http.routers.nitter-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.nitter-insecure.entrypoints: web
      traefik.http.routers.nitter-insecure.service: nitter
      traefik.http.routers.nitter-insecure.middlewares: nitter-web-redirect
      traefik.http.routers.nitter.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.nitter.entrypoints: websecure
      traefik.http.routers.nitter.service: nitter
      traefik.http.routers.nitter.tls.certresolver: myresolver
      traefik.http.routers.nitter-local-insecure.rule: Host(`nitter.${LOCAL_DOMAIN}`)
      traefik.http.routers.nitter-local-insecure.entrypoints: web
      traefik.http.routers.nitter-local-insecure.service: nitter
      traefik.http.routers.nitter-local-insecure.middlewares: nitter-web-redirect
      traefik.http.routers.nitter-local.rule: Host(`nitter.${LOCAL_DOMAIN}`)
      traefik.http.routers.nitter-local.entrypoints: websecure
      traefik.http.routers.nitter-local.service: nitter
      traefik.http.routers.nitter-local.tls: true
      runtipi.managed: true
  nitter-redis:
    image: redis:alpine
    container_name: nitter-redis
    networks:
    - tipi_main_network
    command: redis-server --save 60 1 --loglevel warning
    volumes:
    - ${APP_DATA_DIR}/data/redis:/data
    restart: unless-stopped
    labels:
      runtipi.managed: true
 
```
