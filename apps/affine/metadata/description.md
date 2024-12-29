
# JSON
```json
{
  "services": [
    {
      "name": "affine",
      "image": "ghcr.io/toeverything/affine-graphql:stable",
      "isMain": true,
      "internalPort": 3010,
      "environment": {
        "NODE_OPTIONS": "\"--import",
        "AFFINE_CONFIG_PATH": "/root/.affine/config",
        "REDIS_SERVER_HOST": "affine-redis",
        "DATABASE_URL": "postgres://tipi:${AFFINE_POSTGRES_PASSWORD}@affine-postgres:5432/affine",
        "NODE_ENV": "production",
        "AFFINE_ADMIN_EMAIL": "${AFFINE_ADMIN_EMAIL}",
        "AFFINE_ADMIN_PASSWORD": "${AFFINE_ADMIN_PASSWORD}",
        "TELEMETRY_ENABLE": "${AFFINE_TELEMETRY_ENABLE}"
      },
      "dependsOn": {
        "affine-redis": {
          "condition": "service_healthy"
        },
        "affine-postgres": {
          "condition": "service_healthy"
        }
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/config",
          "containerPath": "/root/.affine/config"
        },
        {
          "hostPath": "${APP_DATA_DIR}/data/storage",
          "containerPath": "/root/.affine/storage"
        }
      ],
      "command": [
        "sh",
        "-c",
        "node ./scripts/self-host-predeploy && node ./dist/index.js"
      ],
      "logging": {
        "driver": "json-file",
        "options": {
          "max-size": "1000m"
        }
      }
    },
    {
      "name": "affine-redis",
      "image": "redis",
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/redis",
          "containerPath": "/data"
        }
      ],
      "healthCheck": {
        "interval": "10s",
        "timeout": "5s",
        "retries": 5,
        "test": "redis-cli --raw incr ping"
      }
    },
    {
      "name": "affine-postgres",
      "image": "postgres:16",
      "environment": {
        "POSTGRES_USER": "tipi",
        "POSTGRES_PASSWORD": "${AFFINE_POSTGRES_PASSWORD}",
        "POSTGRES_DB": "affine"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/postgres",
          "containerPath": "/var/lib/postgresql/data"
        }
      ],
      "healthCheck": {
        "interval": "10s",
        "timeout": "5s",
        "retries": 5,
        "test": "pg_isready -d postgres://tipi:${AFFINE_POSTGRES_PASSWORD}@affine-postgres:5432/affine"
      }
    }
  ]
} 
```
# YAML
```yaml
version: '3.9'
services:
  affine:
    image: ghcr.io/toeverything/affine-graphql:stable
    container_name: affine
    command:
    - sh
    - -c
    - node ./scripts/self-host-predeploy && node ./dist/index.js
    ports:
    - ${APP_PORT}:3010
    depends_on:
      affine-redis:
        condition: service_healthy
      affine-postgres:
        condition: service_healthy
    volumes:
    - ${APP_DATA_DIR}/data/config:/root/.affine/config
    - ${APP_DATA_DIR}/data/storage:/root/.affine/storage
    logging:
      driver: json-file
      options:
        max-size: 1000m
    restart: unless-stopped
    environment:
    - NODE_OPTIONS="--import=./scripts/register.js"
    - AFFINE_CONFIG_PATH=/root/.affine/config
    - REDIS_SERVER_HOST=affine-redis
    - DATABASE_URL=postgres://tipi:${AFFINE_POSTGRES_PASSWORD}@affine-postgres:5432/affine
    - NODE_ENV=production
    - AFFINE_ADMIN_EMAIL=${AFFINE_ADMIN_EMAIL}
    - AFFINE_ADMIN_PASSWORD=${AFFINE_ADMIN_PASSWORD}
    - TELEMETRY_ENABLE=${AFFINE_TELEMETRY_ENABLE}
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.affine-web-redirect.redirectscheme.scheme: https
      traefik.http.services.affine.loadbalancer.server.port: 3010
      traefik.http.routers.affine-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.affine-insecure.entrypoints: web
      traefik.http.routers.affine-insecure.service: affine
      traefik.http.routers.affine-insecure.middlewares: affine-web-redirect
      traefik.http.routers.affine.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.affine.entrypoints: websecure
      traefik.http.routers.affine.service: affine
      traefik.http.routers.affine.tls.certresolver: myresolver
      traefik.http.routers.affine-local-insecure.rule: Host(`affine.${LOCAL_DOMAIN}`)
      traefik.http.routers.affine-local-insecure.entrypoints: web
      traefik.http.routers.affine-local-insecure.service: affine
      traefik.http.routers.affine-local-insecure.middlewares: affine-web-redirect
      traefik.http.routers.affine-local.rule: Host(`affine.${LOCAL_DOMAIN}`)
      traefik.http.routers.affine-local.entrypoints: websecure
      traefik.http.routers.affine-local.service: affine
      traefik.http.routers.affine-local.tls: true
      runtipi.managed: true
  affine-redis:
    image: redis
    container_name: affine-redis
    restart: unless-stopped
    volumes:
    - ${APP_DATA_DIR}/data/redis:/data
    healthcheck:
      test:
      - CMD
      - redis-cli
      - --raw
      - incr
      - ping
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
    - tipi_main_network
    labels:
      runtipi.managed: true
  affine-postgres:
    image: postgres:16
    container_name: affine-postgres
    restart: unless-stopped
    volumes:
    - ${APP_DATA_DIR}/data/postgres:/var/lib/postgresql/data
    healthcheck:
      test:
      - CMD-SHELL
      - pg_isready -d postgres://tipi:${AFFINE_POSTGRES_PASSWORD}@affine-postgres:5432/affine
      interval: 10s
      timeout: 5s
      retries: 5
    environment:
      POSTGRES_USER: tipi
      POSTGRES_PASSWORD: ${AFFINE_POSTGRES_PASSWORD}
      POSTGRES_DB: affine
    networks:
    - tipi_main_network
    labels:
      runtipi.managed: true
 
```
