
# JSON
```json
{
  "services": [
    {
      "name": "gladys",
      "image": "gladysassistant/gladys:v4.50.2",
      "isMain": true,
      "networkMode": "host",
      "environment": {
        "NODE_ENV": "production",
        "SERVER_PORT": "8270",
        "TZ": "${TZ}",
        "SQLITE_FILE_PATH": "/var/lib/gladysassistant/gladys-production.db"
      },
      "volumes": [
        {
          "hostPath": "/var/run/docker.sock",
          "containerPath": "/var/run/docker.sock"
        },
        {
          "hostPath": "${APP_DATA_DIR}/data/gladysassistant",
          "containerPath": "/var/lib/gladysassistant"
        },
        {
          "hostPath": "/dev",
          "containerPath": "/dev"
        },
        {
          "hostPath": "/run/udev",
          "containerPath": "/run/udev",
          "readOnly": true
        }
      ],
      "privileged": true,
      "stop_grace_period": "1m"
    }
  ]
} 
```
# YAML
```yaml
version: '3'
services:
  gladys:
    container_name: gladys
    image: gladysassistant/gladys:v4.50.2
    privileged: true
    restart: on-failure
    stop_grace_period: 1m
    network_mode: host
    environment:
    - NODE_ENV=production
    - SERVER_PORT=8270
    - TZ=${TZ}
    - SQLITE_FILE_PATH=/var/lib/gladysassistant/gladys-production.db
    volumes:
    - /var/run/docker.sock:/var/run/docker.sock
    - ${APP_DATA_DIR}/data/gladysassistant:/var/lib/gladysassistant
    - /dev:/dev
    - /run/udev:/run/udev:ro
    labels:
      runtipi.managed: true
 
```
