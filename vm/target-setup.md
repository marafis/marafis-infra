Target setup:

 - Cloud Domain
- ↓
- Cloud DNS (A records)
- ↓
- VM (Hetzner Public IP / Load Balancer)
- ↓
- Dockerized NGINX (reverse proxy + SSL)
- ↓
- Web / Blog containers
- ↓
- DB (separate internal network)


# First-time SSL issuance (Certbot) - run only once per domain:
```
   docker compose run certbot certonly \
  --webroot \
  --webroot-path=/var/www/certbot \
  --email oncmmr@gmail.com \
  --agree-tos \
  --no-eff-email \
  -d marafis.com -d www.marafis.com
```

Then restart nginx:
```
docker compose restart nginx
```

Auto-renew SSL (IMPORTANT)
Add cron on VM:
```
0 3 * * * docker compose run certbot renew && docker compose restart nginx
```