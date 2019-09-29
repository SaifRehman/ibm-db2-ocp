# ibm-db2-ocp

## Steps

1. clone the rep
```
$ git clone https://github.com/SaifRehman/ibm-db2-ocp.git
```

2. Set HELM

* Tiller configuration
```
$ oc new-project tiller
$ oc project tiller
$ export TILLER_NAMESPACE=tiller
```

* If linux
```
$ curl -s https://storage.googleapis.com/kubernetes-helm/helm-v2.9.0-linux-amd64.tar.gz | tar xz
$ cd linux-amd64
$ ./helm init --client-only
```

* If Mac
```
$ curl -s https://storage.googleapis.com/kubernetes-helm/helm-v2.9.0-darwin-amd64.tar.gz | tar xz
$ cd darwin-amd64
$ ./helm init --client-only
```

* Deploy Tiller
```
$ oc process -f https://github.com/openshift/origin/raw/master/examples/helm/tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -p HELM_VERSION=v2.9.0 | oc create -f -
```

3. Create namespace and deploy 

```
$ oc new-project tenant
$ oc policy add-role-to-user edit "system:serviceaccount:${TILLER_NAMESPACE}:tiller"
```

4. Set service account permissions 

* Create admin service account 
```
$ oc create serviceaccount -n {namespace} {adminusername}
$ oc adm policy add-scc-to-user privileged -n {namespace} -z default
```

5. Deploy

```
$ helm install ibm-db2oltp-dev/ --name="db2" --namespace="tenant"
```