# K3S : Bind9 DNS : Fluent-Bit : Azure Log Ingestion API
A K3S Deployment of Bind9 DNS, Fluent-Bit configured for the [output] Azure Log Ingestion API

## Pre-Condition
* [Microsoft Azure Subscription](https://azure.microsoft.com/en-us/free/search/?ef_id=_k_Cj0KCQiAsvWrBhC0ARIsAO4E6f_KQHuZoJWDcf25quxjVXhxQz0NixXwZOXDb38WnIOyANw1wRbw6qwaAs6AEALw_wcB_k_&OCID=AIDcmm5edswduu_SEM__k_Cj0KCQiAsvWrBhC0ARIsAO4E6f_KQHuZoJWDcf25quxjVXhxQz0NixXwZOXDb38WnIOyANw1wRbw6qwaAs6AEALw_wcB_k_&gad_source=1&gclid=Cj0KCQiAsvWrBhC0ARIsAO4E6f_KQHuZoJWDcf25quxjVXhxQz0NixXwZOXDb38WnIOyANw1wRbw6qwaAs6AEALw_wcB)
* Operational [K3S Node/Cluster](https://www.youtube.com/watch?v=hT2_O2Yd_wE&t=47s)
  ![image](https://github.com/dcodev1702/k3s_bind9_dns_fluentbit_azure_log_ingest_api/assets/32214072/3d406260-aacb-42f5-8e82-0d2f84c62571)

* [Fluent Bit: Azure Log Ingestion API](https://docs.fluentbit.io/manual/pipeline/outputs/azure_logs_ingestion)
* bind9-dns namespace must exist
  ```console
  kubectl create ns bind9-dns
  ```
* A label of 'bind9' : 'true' must exist on a K3S node for 'nodeSelector'
  ```console
  kubectl label nodes <your-node-name> bind9=true
  kubectl get nodes --show-labels
  ```
  * This is required for two containers to write/read from the same volume mount set as RWO (Read Write Once)
    * Source: [Longhorn Volumes](https://support.tools/post/fixing-longhorn-volume-issues/#:~:text=Longhorn%20supports%20RWO%20(ReadWriteOnce)%20volumes,only%20have%20a%20few%20options.)
  * Both container MUST run on the SAME K3S (Kubernetes) NODE
* Kubernetes storage (e.g. LongHorn) must exist for K3S node/cluster
  * Volume name: bind9-logs set as RWO
    ![image](https://github.com/dcodev1702/k3s_bind9_dns_fluentbit_azure_log_ingest_api/assets/32214072/08d03116-ed6e-422f-a6ad-53be4cfd8287)

* K3S Load Balancer must be configured (e.g. KubeVIP)
  

## Order of Operations
* Add your info & secrets from Azure Entra ID (App Registration) & Azure Monitor (DCE/DCR) and deploy the manifest
```console
kubectl create -f ./bind9-appreg-secrets.yaml
```
* Modify this manifest & deploy the Fluent-Bit Sidecar ConfigMap
```console
kubectl create -f ./fluentbit-sidecar-cm.yaml
```
* Modify this manifest & deploy Bind9 DNS w/ Fluent-Bit Sidecar
```console
kubectl create -f ./bind9-fb-sc-deployment.yaml
``` 
