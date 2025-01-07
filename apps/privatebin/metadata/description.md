# Checklist
## Dynamic compose for privatebin
This is a privatebin update for using dynamic compose.
##### Reaching the app :
- [ ] http://localip:port
- [ ] https://privatebin.tipi.local
##### In app tests :
- [ ] 📝 Register and log in
- [ ] 🖱 Basic interaction
- [ ] 🌆 Uploading data
- [ ] 🔄 Check data after restart
##### Volumes mapping :
- [ ] ${APP_DATA_DIR}/data:/srv/data
##### Specific instructions :
- 🏷 DNS (skipped)

# New JSON
```json
{
  "$schema": "../dynamic-compose-schema.json",
  "services": [
    {
      "name": "privatebin",
      "image": "privatebin/nginx-fpm-alpine:1.7.5",
      "isMain": true,
      "internalPort": 8080,
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data",
          "containerPath": "/srv/data"
        }
      ]
    }
  ]
} 
```
# Original YAML
```yaml
version: '3.7'
services:
  privatebin:
    image: privatebin/nginx-fpm-alpine:1.7.5
    container_name: privatebin
    dns:
    - ${DNS_IP}
    ports:
    - ${APP_PORT}:8080
    restart: unless-stopped
    networks:
    - tipi_main_network
    volumes:
    - ${APP_DATA_DIR}/data:/srv/data
    labels:
      traefik.enable: true
      traefik.http.middlewares.privatebin-web-redirect.redirectscheme.scheme: https
      traefik.http.services.privatebin.loadbalancer.server.port: 8080
      traefik.http.routers.privatebin-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.privatebin-insecure.entrypoints: web
      traefik.http.routers.privatebin-insecure.service: privatebin
      traefik.http.routers.privatebin-insecure.middlewares: privatebin-web-redirect
      traefik.http.routers.privatebin.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.privatebin.entrypoints: websecure
      traefik.http.routers.privatebin.service: privatebin
      traefik.http.routers.privatebin.tls.certresolver: myresolver
      traefik.http.routers.privatebin-local-insecure.rule: Host(`privatebin.${LOCAL_DOMAIN}`)
      traefik.http.routers.privatebin-local-insecure.entrypoints: web
      traefik.http.routers.privatebin-local-insecure.service: privatebin
      traefik.http.routers.privatebin-local-insecure.middlewares: privatebin-web-redirect
      traefik.http.routers.privatebin-local.rule: Host(`privatebin.${LOCAL_DOMAIN}`)
      traefik.http.routers.privatebin-local.entrypoints: websecure
      traefik.http.routers.privatebin-local.service: privatebin
      traefik.http.routers.privatebin-local.tls: true
      runtipi.managed: true
 
```
