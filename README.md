# infrastructure
The infrastructure code used to deploy staples.

# pre-req

Secret must exist.

# adding the cert secret

```
kubectl create secret generic staple-certs --from-file=server.key=/path/to/.ssh/server.key --from-file=server.crt=/path/to/.ssh/server.crt -n staple
```