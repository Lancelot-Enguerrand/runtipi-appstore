# Checklist
## Dynamic compose for cloudflare-ddns
This is a cloudflare-ddns update for using dynamic compose.
##### Reaching the app :
##### In app tests :
- [ ] 📝 Register and log in
- [ ] 🖱 Basic interaction
- [ ] 🌆 Uploading data
- [ ] 🔄 Check data after restart
##### Specific instructions :
- [ ] 🌳 Environment

# New JSON
```json
{
  "$schema": "../dynamic-compose-schema.json",
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
# Original YAML
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
