# caddy

Start docker container:
```bash
docker run -d -p 80:80 -p 443:443 \
  -v ${PWD}/Caddyfile:/etc/caddy/Caddyfile \
  -v ${PWD}/data:/data \
  --name caddy \
  caddy
```
