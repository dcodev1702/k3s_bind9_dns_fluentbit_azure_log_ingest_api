# K3S : Bind9 DNS : Fluent-Bit : Azure Log Ingestion API
A K3S Deployment of Bind9 DNS, Fluent-Bit configured for the [output] Azure Log Ingestion API

## Pre-Condition
* bind9-dns namespace must exist
* A label of 'bind9' : 'true' must exist on a K3S node for 'nodeSelector'
  * This is required for two containers to write/read from the same volume mount set as RWO (Read Write Once)
  * Both container MUST run on the SAME K3S (Kubernetes) NODE
* Kubernetes storage (e.g. LongHorn) must exist for K3S node/cluster
  * Volume name: bind9-logs set as RWO

## Order of Operations
* Setup your secrets and deploy the manifest
```console
kubectl create -f ./bind9-secrets.yaml
```
* Deploy the Fluent-Bit Sidecar ConfigMap
```console
kubectl create -f ./fluentbit-sidecar-cm.yaml
```
* Deploy Bind9 DNS w/ Fluent-Bit Sidecar
```console
kubectl create -f ./bind9-deployment.yaml
``` 
