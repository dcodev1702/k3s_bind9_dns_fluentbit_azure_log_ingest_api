# K3S : Bind9 DNS : Fluent-Bit : Azure Log Ingestion API
A K3S Deployment of Bind9 DNS, Fluent-Bit configured for the [output] Azure Log Ingestion API

## Pre-Condition
* [Fluent Bit: Azure Log Ingestion API](https://docs.fluentbit.io/manual/pipeline/outputs/azure_logs_ingestion)
* bind9-dns namespace must exist
* A label of 'bind9' : 'true' must exist on a K3S node for 'nodeSelector'
  * This is required for two containers to write/read from the same volume mount set as RWO (Read Write Once)
  * Both container MUST run on the SAME K3S (Kubernetes) NODE
* Kubernetes storage (e.g. LongHorn) must exist for K3S node/cluster
  * Volume name: bind9-logs set as RWO
* K3S Load Balancer must be configured (e.g. KubeVIP)
  

## Order of Operations
* Setup your info & secrets from Azure Entra ID (App Registration) & Azure Monitor (DCE/DCR) and deploy the manifest
```console
kubectl create -f ./bind9-appreg-secrets.yaml
```
* Deploy the Fluent-Bit Sidecar ConfigMap
```console
kubectl create -f ./fluentbit-sidecar-cm.yaml
```
* Deploy Bind9 DNS w/ Fluent-Bit Sidecar
```console
kubectl create -f ./bind9-fb-sc-deployment.yaml
``` 
