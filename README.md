# infrastructure
The infrastructure code used to deploy staples.

# pre-req

Secret must exist.

# adding the cert secret

```
kubectl create secret generic staple-certs --from-file=server.key=/path/to/.ssh/server.key --from-file=server.crt=/path/to/.ssh/server.crt -n staple
```

# Ingress sample setup of the two different services on the same domain

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  namespace: staple
  name: staple-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    cert-manager.io/acme-challenge-type: http01
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - staple.cronohub.org
    secretName: staple-tls
  rules:
  - host: staple.cronohub.org
    http:
      paths:
      - backend:
          serviceName: staple-service
          servicePort: 9998
        path: /(rest/api/1.*)
  - host: staple.cronohub.org
    http:
      paths:
      - backend:
          serviceName: staple-front-service
          servicePort: 5000
        path: /(.*)
```
