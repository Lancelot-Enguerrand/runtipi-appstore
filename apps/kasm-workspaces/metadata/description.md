
# JSON
```json
{
  "services": [
    {
      "name": "kasm-workspaces",
      "image": "lscr.io/linuxserver/kasm:1.120.20221218",
      "isMain": true,
      "internalPort": "${APP_PORT}",
      "addPorts": [
        {
          "hostPort": 8743,
          "containerPort": 3000
        }
      ],
      "environment": {
        "KASM_PORT": "${APP_PORT}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data",
          "containerPath": "/opt"
        }
      ],
      "privileged": true
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  kasm-workspaces:
    image: lscr.io/linuxserver/kasm:1.120.20221218
    container_name: kasm-workspaces
    privileged: true
    environment:
    - KASM_PORT=${APP_PORT}
    volumes:
    - ${APP_DATA_DIR}/data:/opt
    ports:
    - 8743:3000
    - ${APP_PORT}:${APP_PORT}
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: false
      runtipi.managed: true
 
```
