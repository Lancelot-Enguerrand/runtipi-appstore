
# JSON
```json
{
  "services": [
    {
      "name": "chatgpt-ui",
      "image": "wongsaang/chatgpt-ui-client:v2.5.9",
      "isMain": true,
      "addPorts": [
        {
          "hostPort": 8200,
          "containerPort": 80
        }
      ],
      "environment": {
        "SERVER_DOMAIN": "http://chatgpt-ui-web-server",
        "DEFAULT_LOCALE": "en"
      },
      "dependsOn": [
        "chatgpt-ui-web-server"
      ]
    },
    {
      "name": "chatgpt-ui-wsgi-server",
      "image": "wongsaang/chatgpt-ui-wsgi-server:v2.5.2",
      "environment": {
        "APP_DOMAIN": "${INTERNAL_IP}:8201",
        "SERVER_WORKERS": "3",
        "WORKER_TIMEOUT": "180",
        "DB_URL": "mysql://tipi:${CHATGPT_UI_DB_PASSWORD}@chatgpt-ui-db:3306/chatgptdb",
        "DJANGO_SUPERUSER_USERNAME": "${CHATGPT_UI_SUPERUSER_USERNAME}",
        "DJANGO_SUPERUSER_PASSWORD": "${CHATGPT_UI_SUPERUSER_PASSWORD}",
        "DJANGO_SUPERUSER_EMAIL": "${CHATGPT_UI_SUPERUSER_EMAIL}",
        "ACCOUNT_EMAIL_VERIFICATION": "none"
      },
      "dependsOn": [
        "chatgpt-ui-db"
      ]
    },
    {
      "name": "chatgpt-ui-web-server",
      "image": "wongsaang/chatgpt-ui-web-server:v2.5.2",
      "addPorts": [
        {
          "hostPort": 8201,
          "containerPort": 80
        }
      ],
      "environment": {
        "BACKEND_URL": "http://chatgpt-ui-wsgi-server:8000"
      },
      "dependsOn": [
        "chatgpt-ui-wsgi-server",
        "chatgpt-ui-db"
      ]
    },
    {
      "name": "chatgpt-ui-db",
      "image": "lscr.io/linuxserver/mariadb:latest",
      "environment": {
        "MYSQL_ROOT_PASSWORD": "$CHATGPT_UI_DB_PASSWORD}",
        "MYSQL_DATABASE": "chatgptdb",
        "MYSQL_USER": "tipi",
        "MYSQL_PASSWORD": "${CHATGPT_UI_DB_PASSWORD}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/mysql/config",
          "containerPath": "/config"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '3'
services:
  chatgpt-ui:
    image: wongsaang/chatgpt-ui-client:v2.5.9
    container_name: chatgpt-ui
    environment:
    - SERVER_DOMAIN=http://chatgpt-ui-web-server
    - DEFAULT_LOCALE=en
    depends_on:
    - chatgpt-ui-web-server
    ports:
    - 8200:80
    restart: always
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.chatgpt-ui-web-redirect.redirectscheme.scheme: https
      traefik.http.services.chatgpt-ui.loadbalancer.server.port: 80
      traefik.http.routers.chatgpt-ui-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.chatgpt-ui-insecure.entrypoints: web
      traefik.http.routers.chatgpt-ui-insecure.service: chatgpt-ui
      traefik.http.routers.chatgpt-ui-insecure.middlewares: chatgpt-ui-web-redirect
      traefik.http.routers.chatgpt-ui.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.chatgpt-ui.entrypoints: websecure
      traefik.http.routers.chatgpt-ui.service: chatgpt-ui
      traefik.http.routers.chatgpt-ui.tls.certresolver: myresolver
      traefik.http.routers.chatgpt-ui-local-insecure.rule: Host(`chatgpt-ui.${LOCAL_DOMAIN}`)
      traefik.http.routers.chatgpt-ui-local-insecure.entrypoints: web
      traefik.http.routers.chatgpt-ui-local-insecure.service: chatgpt-ui
      traefik.http.routers.chatgpt-ui-local-insecure.middlewares: chatgpt-ui-web-redirect
      traefik.http.routers.chatgpt-ui-local.rule: Host(`chatgpt-ui.${LOCAL_DOMAIN}`)
      traefik.http.routers.chatgpt-ui-local.entrypoints: websecure
      traefik.http.routers.chatgpt-ui-local.service: chatgpt-ui
      traefik.http.routers.chatgpt-ui-local.tls: true
      runtipi.managed: true
  chatgpt-ui-wsgi-server:
    image: wongsaang/chatgpt-ui-wsgi-server:v2.5.2
    container_name: chatgpt-ui-wsgi-server
    environment:
    - APP_DOMAIN=${INTERNAL_IP}:8201
    - SERVER_WORKERS=3
    - WORKER_TIMEOUT=180
    - DB_URL=mysql://tipi:${CHATGPT_UI_DB_PASSWORD}@chatgpt-ui-db:3306/chatgptdb
    - DJANGO_SUPERUSER_USERNAME=${CHATGPT_UI_SUPERUSER_USERNAME}
    - DJANGO_SUPERUSER_PASSWORD=${CHATGPT_UI_SUPERUSER_PASSWORD}
    - DJANGO_SUPERUSER_EMAIL=${CHATGPT_UI_SUPERUSER_EMAIL}
    - ACCOUNT_EMAIL_VERIFICATION=none
    networks:
    - tipi_main_network
    depends_on:
    - chatgpt-ui-db
    restart: always
    labels:
      runtipi.managed: true
  chatgpt-ui-web-server:
    image: wongsaang/chatgpt-ui-web-server:v2.5.2
    container_name: chatgpt-ui-web-server
    environment:
    - BACKEND_URL=http://chatgpt-ui-wsgi-server:8000
    ports:
    - 8201:80
    depends_on:
    - chatgpt-ui-wsgi-server
    - chatgpt-ui-db
    networks:
    - tipi_main_network
    restart: always
    labels:
      runtipi.managed: true
  chatgpt-ui-db:
    container_name: chatgpt-ui-db
    image: lscr.io/linuxserver/mariadb:latest
    environment:
    - MYSQL_ROOT_PASSWORD=$CHATGPT_UI_DB_PASSWORD}
    - MYSQL_DATABASE=chatgptdb
    - MYSQL_USER=tipi
    - MYSQL_PASSWORD=${CHATGPT_UI_DB_PASSWORD}
    volumes:
    - ${APP_DATA_DIR}/data/mysql/config:/config
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      runtipi.managed: true
 
```
