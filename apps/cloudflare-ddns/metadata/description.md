
# JSON
```json
{
  "services": [
    {
      "name": "cloudflare-ddns",
      "image": "ghcr.io/joshuaavalon/cloudflare-ddns:3.3.0",
      "isMain": true,
      "environment": {
        "CF_DNS__AUTH__SCOPED_TOKEN": "${CF_DNS__AUTH__SCOPED_TOKEN}",
        "CF_DNS__DOMAINS_0__NAME": "${CF_DNS__DOMAINS_0__NAME}",
        "CF_DNS__DOMAINS_0__PROXIED": "false",
        "CF_DNS__LOG_TYPE": "json"
      }
    }
  ]
} 
```
# YAML
```yaml
version: '3.9'
services:
  cloudflare-ddns:
    container_name: cloudflare-ddns
    image: ghcr.io/joshuaavalon/cloudflare-ddns:3.3.0
    environment:
    - CF_DNS__AUTH__SCOPED_TOKEN=${CF_DNS__AUTH__SCOPED_TOKEN}
    - CF_DNS__DOMAINS_0__NAME=${CF_DNS__DOMAINS_0__NAME}
    - CF_DNS__DOMAINS_0__PROXIED=false
    - CF_DNS__LOG_TYPE=json
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      runtipi.managed: true
 
```
