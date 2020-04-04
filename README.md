# Kustomization for Deploying Red Hat OpenShift Service Mesh

Red Hat OpenShift Service Mesh product documentation can be found [here](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.3/html/service_mesh/index). This kustomization is based on the examples included in the product documentation.

You can refer to the [Maistra Istio Operator](https://github.com/Maistra/istio-operator) project on GitHub for further documentation on the operator.

## Deploying required operators

```
$ oc apply --kustomize elasticsearch-operator/base
```

```
$ oc apply --kustomize jaeger-operator/base
```

```
$ oc apply --kustomize kiali-operator/base
```

```
$ oc apply --kustomize service-mesh-operator/base
```

You should now see operators running in the *openshift-operators* project:

```
oc get pod --namespace openshift-operators
NAME                                     READY   STATUS    RESTARTS   AGE
elasticsearch-operator-6b4686b59-fz6cx   1/1     Running   0          11h
istio-operator-5f945bd597-z89qp          1/1     Running   0          11h
jaeger-operator-54b947db5d-lck5w         1/1     Running   0          11h
kiali-operator-6559fdc5bc-vspjd          1/1     Running   0          11h
```

## Deploying Service Mesh control plane

Install service mesh control plane:

```
$ oc apply --kustomize service-mesh-instance/overlays/development
```

Discover the service mesh endpoint URLs:
```
$ oc get route --namespace istio-system
```

## Deploying Booinfo sample application

```
$ oc new-project bookinfo
```

```
$ oc patch \
    smmr default \
    --namespace istio-system \
    --type json \
    --patch '[{"op": "add", "path": "/spec/members", "value":["bookinfo"]}]'
```
