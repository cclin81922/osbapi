# For local development with Docker-for-Mac K8s

First, bring up baas

```
helm init

go get -u github.com/cclin81922/osbapi-baas/cmd/osbapibaas
cd ~/go/src/github.com/cclin81922/osbapi-baas
make deploy-baas

export POD_NAME=$(kubectl get pods --namespace baas-skeleton -l "app=osbapibaas,release=baas-skeleton" -o jsonpath="{.items[0].metadata.name}")
kubectl -n baas-skeleton logs $POD_NAME

# 2018/10/15 02:06:06 Base url is https://localhost.localdomain:443/
```

Second, bring up broker

```
go get -u github.com/cclin81922/osb-starter-pack/cmd/servicebroker
cd ~/go/src/github.com/cclin81922/osb-starter-pack
make deploy-sc
IMAGE=cclin81922/osbapi-broker TAG=latest PULL=Never make deploy-broker
```

Finally, bring up app

```
go get -u github.com/cclin81922/osbapi-app/cmd/osbapiapp
cd ~/go/src/github.com/cclin81922/osbapi-app
make provision-svc
make bind-svc
make TAG=latest PULL=Never make deploy-app
```

Verify

```
kubectl -n app-skeleton get pod
kubectl -n app-skeleton logs <app-skeleton pod>
```

# For test with GKE

TODO

# Related Projects

* [osbapi-app](https://github.com/cclin81922/osbapi-app)
* [osbapi-sdk](https://github.com/cclin81922/osbapi-sdk)
* [osbapi-broker](https://github.com/cclin81922/osbapi-broker)
* [osbapi-baas](https://github.com/cclin81922/osbapi-baas)

# Related Resources

* [Open Service Broker API](https://www.openservicebrokerapi.org/)
* [Kubernetes Service Catalog](https://svc-cat.io/)
