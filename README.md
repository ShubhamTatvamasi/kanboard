# kanboard

deploy kanboard
```bash
kubectl create deployment kanboard --image=kanboard/kanboard
kubectl expose deployment kanboard --port=80 --name=kanboard
```
> user: `admin` pass: `admin`

delete everything:
```bash
kubectl delete deploy/kanboard svc/kanboard ing/kanboard
```

create ingress value:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: kanboard
  annotations:
    nginx.org/websocket-services: kanboard
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
    - hosts:
      - kanboard.k8s.shubhamtatvamasi.com
      secretName: kanboard-tls
  rules:
    - host: kanboard.k8s.shubhamtatvamasi.com
      http:
        paths:
        - path: /
          backend:
            serviceName: kanboard
            servicePort: 80
EOF
```
