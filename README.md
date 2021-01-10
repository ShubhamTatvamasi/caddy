# caddy

Start docker container:
```bash
docker run -d -p 80:80 -p 443:443 \
  -v ${PWD}/Caddyfile:/etc/caddy/Caddyfile \
  -v ${PWD}/data:/data \
  --name caddy \
  caddy
```

Add ingress value in kubernetes:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: nginx
spec:
  rules:
    - host: nginx.k8s.shubhamtatvamasi.com
      http:
        paths:
        - path: /
          backend:
            serviceName: nginx
            servicePort: 80
EOF
```
