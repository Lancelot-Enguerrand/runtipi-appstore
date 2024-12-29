
# JSON
```json
{
  "services": [
    {
      "name": "send",
      "image": "registry.gitlab.com/timvisee/send:latest",
      "isMain": true,
      "internalPort": 1443,
      "environment": {
        "NODE_ENV": "production",
        "BASE_URL": "${APP_PROTOCOL:-http}://${APP_DOMAIN}",
        "REDIS_HOST": "send-redis",
        "MAX_FILE_SIZE": "2147483648",
        "FILE_DIR": "/uploads"
      },
      "dependsOn": [
        "send-redis"
      ],
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/uploads",
          "containerPath": "/uploads"
        }
      ]
    },
    {
      "name": "send-redis",
      "image": "redis:alpine",
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/redis",
          "containerPath": "/data"
        }
      ],
      "healthCheck": {
        "interval": "5s",
        "timeout": "3s",
        "retries": 30,
        "test": "redis-cli ping"
      }
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  send:
    image: registry.gitlab.com/timvisee/send:latest
    container_name: send
    restart: unless-stopped
    dns:
    - ${DNS_IP}
    ports:
    - ${APP_PORT}:1443
    networks:
    - tipi_main_network
    volumes:
    - ${APP_DATA_DIR}/data/uploads:/uploads
    environment:
    - NODE_ENV=production
    - BASE_URL=${APP_PROTOCOL:-http}://${APP_DOMAIN}
    - REDIS_HOST=send-redis
    - MAX_FILE_SIZE=2147483648
    - FILE_DIR=/uploads
    depends_on:
    - send-redis
    labels:
      traefik.enable: true
      traefik.http.middlewares.send-web-redirect.redirectscheme.scheme: https
      traefik.http.services.send.loadbalancer.server.port: 1443
      traefik.http.routers.send-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.send-insecure.entrypoints: web
      traefik.http.routers.send-insecure.service: send
      traefik.http.routers.send-insecure.middlewares: send-web-redirect
      traefik.http.routers.send.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.send.entrypoints: websecure
      traefik.http.routers.send.service: send
      traefik.http.routers.send.tls.certresolver: myresolver
      traefik.http.routers.send-local-insecure.rule: Host(`send.${LOCAL_DOMAIN}`)
      traefik.http.routers.send-local-insecure.entrypoints: web
      traefik.http.routers.send-local-insecure.service: send
      traefik.http.routers.send-local-insecure.middlewares: send-web-redirect
      traefik.http.routers.send-local.rule: Host(`send.${LOCAL_DOMAIN}`)
      traefik.http.routers.send-local.entrypoints: websecure
      traefik.http.routers.send-local.service: send
      traefik.http.routers.send-local.tls: true
      runtipi.managed: true
  send-redis:
    image: redis:alpine
    container_name: send-redis
    restart: unless-stopped
    volumes:
    - ${APP_DATA_DIR}/data/redis:/data
    healthcheck:
      test:
      - CMD
      - redis-cli
      - ping
      interval: 5s
      timeout: 3s
      retries: 30
    networks:
    - tipi_main_network
    labels:
      runtipi.managed: true
 
```
