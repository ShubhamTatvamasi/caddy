# caddy

### Ingress

Add caddy-ingress helm repo:
```bash
helm repo add caddy-ingress https://caddyserver.github.io/ingress
helm repo update
```

Install caddy-ingress-controller:
```bash
helm upgrade -i caddy-ingress-controller caddy-ingress/caddy-ingress-controller \
  --version 1.0.4 \
  --create-namespace \
  --namespace caddy-system
```

Change service to ClusterIP:
```bash
kubectl -n caddy-system patch svc caddy-ingress-controller \
  --patch='{"spec": {"type": "ClusterIP"}}'
```

### Test

Create a nginx pod:
```bash
kubectl run nginx --image nginx:alpine
kubectl expose pod nginx --port=80 --name=nginx
```

Create Ingress:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx
  annotations:
    kubernetes.io/ingress.class: caddy
spec:
  rules:
    - host: "nginx.frp.shubhamtatvamasi.com"
      http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: nginx
              port:
                number: 80
EOF
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
