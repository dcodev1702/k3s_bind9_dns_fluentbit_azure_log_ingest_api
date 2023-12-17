# K3S : Bind9 DNS : Fluent-Bit : Azure Log Ingestion API
A K3S Deployment of Bind9 DNS, Fluent-Bit configured for the [output] Azure Log Ingestion API

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
