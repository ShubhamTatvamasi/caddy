# caddy

### Ingress


Add caddy-ingress helm repo:
```bash
helm repo add caddy-ingress https://caddyserver.github.io/ingress
helm repo update
```


---

For custom builds with dns: \
https://caddyserver.com/download

deployment with LoadBalancer
```bash
kubectl create deployment caddy --image=caddy
kubectl expose deployment caddy --port=80 --name=caddy --type=LoadBalancer
```

Start docker container:
```bash
docker run -d -p 80:80 -p 443:443 \
  -v ${PWD}/Caddyfile:/etc/caddy/Caddyfile \
  -v ${PWD}/data:/data \
  --name caddy \
  caddy
```

Deploy on Kubernetes:
```bash
kubectl run caddy --image=caddy --port=80 --expose
```

Add ingress value in kubernetes:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: caddy
spec:
  tls:
    - hosts:
      - caddy.k8s.shubhamtatvamasi.com
      secretName: letsencrypt
  rules:
    - host: caddy.k8s.shubhamtatvamasi.com
      http:
        paths:
        - path: /
          backend:
            serviceName: caddy
            servicePort: 80
EOF
```
