# caddy

Create config file
```bash
cat << EOF > Caddyfile
k8s.shubhamtatvamasi.com,
*.k8s.shubhamtatvamasi.com {
    reverse_proxy 172.21.107.235:80
}
EOF
```

Start docker container:
```bash
docker run -d -p 80:80 -p 443:443 \
  -v $PWD/Caddyfile:/etc/caddy/Caddyfile \
  -v caddy_data:/data \
  caddy
```
