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

svcat get brokers

#   NAME   NAMESPACE   URL   STATUS  
# +------+-----------+-----+--------+

WAIT UNTIL SC READY

make deploy-broker

svcat get brokers broker-skeleton

#        NAME         NAMESPACE                                      URL                                      STATUS  
# +-----------------+-----------+---------------------------------------------------------------------------+--------+
#   broker-skeleton               https://broker-skeleton-broker-skeleton.broker-skeleton.svc.cluster.local   Ready   

WAIT UNTIL BROKER READY
```

Finally, bring up app

```
go get -u github.com/cclin81922/osbapi-app/cmd/osbapiapp
cd ~/go/src/github.com/cclin81922/osbapi-app
make provision-svc
make bind-svc

kubectl get secrets osbapi-app-secret -n app-skeleton

# NAME                  TYPE                                  DATA      AGE
# osbapi-app-secret     Opaque                                6         18s

make deploy-app

export POD_NAME=$(kubectl get pods --namespace app-skeleton -l "app=osbapiapp,release=app-skeleton" -o jsonpath="{.items[0].metadata.name}")
kubectl -n app-skeleton logs $POD_NAME

# 2018/10/15 02:48:46 2018-10-15 02:48:46.154432755 +0000 UTC m=+3.001906152
```

# For test with GKE

TODO

Fisrt, bring up a GKE cluster

```
gcloud container clusters create --num-nodes 2
kubectl apply -f manifests/service-account-helm.yaml
helm init --service-account helm
```


# Related Projects

* [osbapi-app](https://github.com/cclin81922/osbapi-app)
* [osbapi-sdk](https://github.com/cclin81922/osbapi-sdk)
* [osbapi-broker](https://github.com/cclin81922/osbapi-broker)
* [osbapi-baas](https://github.com/cclin81922/osbapi-baas)

# Related Resources

* [Open Service Broker API](https://www.openservicebrokerapi.org/)
* [Kubernetes Service Catalog](https://svc-cat.io/)
