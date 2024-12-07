
# JSON
```json
{
  "services": [
    {
      "name": "matter-server",
      "image": "ghcr.io/home-assistant-libs/python-matter-server:6.6.1",
      "isMain": true,
      "networkMode": "host",
      "volumes": [
        {
          "hostPath": "/run/dbus",
          "containerPath": "/run/dbus",
          "readOnly": true
        },
        {
          "hostPath": "${APP_DATA_DIR}/data",
          "containerPath": "/data"
        }
      ],
      "securityOpt": [
        "apparmor=unconfined"
      ]
    }
  ]
} 
```
# YAML
```yaml
services:
  matter-server:
    image: ghcr.io/home-assistant-libs/python-matter-server:6.6.1
    container_name: matter-server
    restart: unless-stopped
    network_mode: host
    security_opt:
    - apparmor=unconfined
    volumes:
    - /run/dbus:/run/dbus:ro
    - ${APP_DATA_DIR}/data:/data
    labels:
      traefik.enable: false
      runtipi.managed: true
 
```
