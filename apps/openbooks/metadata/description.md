# Checklist
## Dynamic compose for openbooks
This is a openbooks update for using dynamic compose.
##### Reaching the app :
- [ ] http://localip:port
- [ ] https://openbooks.tipi.local
##### In app tests :
- [ ] 📝 Register and log in
- [ ] 🖱 Basic interaction
- [ ] 🌆 Uploading data
- [ ] 🔄 Check data after restart
##### Volumes mapping :
- [ ] ${ROOT_FOLDER_HOST}/media/data/books/:/books
##### Specific instructions :
- [ ] 🌳 Environment
- [ ] ⌨ Command

# New JSON
```json
{
  "$schema": "../dynamic-compose-schema.json",
  "services": [
    {
      "name": "openbooks",
      "image": "evanbuss/openbooks:4.5.0",
      "isMain": true,
      "internalPort": 80,
      "environment": {
        "BASE_PATH": "/openbooks/"
      },
      "volumes": [
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/data/books/",
          "containerPath": "/books"
        }
      ],
      "command": "./openbooks server --dir /books --port 80 --persist --name ${OPENBOOKS_IRC_USERNAME}"
    }
  ]
} 
```
# Original YAML
```yaml
version: '3'
services:
  openbooks:
    container_name: openbooks
    image: evanbuss/openbooks:4.5.0
    command: ./openbooks server --dir /books --port 80 --persist --name ${OPENBOOKS_IRC_USERNAME}
    ports:
    - ${APP_PORT}:80
    volumes:
    - ${ROOT_FOLDER_HOST}/media/data/books/:/books
    environment:
    - BASE_PATH=/openbooks/
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.openbooks-web-redirect.redirectscheme.scheme: https
      traefik.http.services.openbooks.loadbalancer.server.port: 80
      traefik.http.routers.openbooks-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.openbooks-insecure.entrypoints: web
      traefik.http.routers.openbooks-insecure.service: openbooks
      traefik.http.routers.openbooks-insecure.middlewares: openbooks-web-redirect
      traefik.http.routers.openbooks.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.openbooks.entrypoints: websecure
      traefik.http.routers.openbooks.service: openbooks
      traefik.http.routers.openbooks.tls.certresolver: myresolver
      traefik.http.routers.openbooks-local-insecure.rule: Host(`openbooks.${LOCAL_DOMAIN}`)
      traefik.http.routers.openbooks-local-insecure.entrypoints: web
      traefik.http.routers.openbooks-local-insecure.service: openbooks
      traefik.http.routers.openbooks-local-insecure.middlewares: openbooks-web-redirect
      traefik.http.routers.openbooks-local.rule: Host(`openbooks.${LOCAL_DOMAIN}`)
      traefik.http.routers.openbooks-local.entrypoints: websecure
      traefik.http.routers.openbooks-local.service: openbooks
      traefik.http.routers.openbooks-local.tls: true
      runtipi.managed: true
 
```
