#########################################################################################
# ADDENDA 0: Commands good to know
#########################################################################################

Here, I will bring you a few useful commands that you can use sometimes.  


## A. How can I easily list all the containers in a POD

As there is no option to do so, you need to _extract_ this information from the POD definition.
Here are some examples with Trident 20.01.1, in several Kubernetes versions
```
# 1.15 
# kubectl get pods -n trident -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.containers[*]}{.image}{", "}{end}{end}' |sort
trident-csi-754c485784-wpbpg:   netapp/trident:20.01.1, quay.io/k8scsi/csi-provisioner:v1.5.0, quay.io/k8scsi/csi-attacher:v2.1.0,
trident-csi-8j74m:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,
trident-csi-fd5qh:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,
trident-csi-xfpwd:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,

# 1.16
# kubectl get pods -n trident -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.containers[*]}{.image}{", "}{end}{end}' |sort
trident-csi-647jc:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,
trident-csi-cvb9p:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,
trident-csi-d8667b7fd-lvddh:    netapp/trident:20.01.1, quay.io/k8scsi/csi-provisioner:v1.5.0, quay.io/k8scsi/csi-attacher:v2.1.0, quay.io/k8scsi/csi-resizer:v0.4.0,
trident-csi-tnf79:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,

#1.17
# kubectl get pods -n trident -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.containers[*]}{.image}{", "}{end}{end}' |sort
trident-csi-5cd46cff6-b5rtr:    netapp/trident:20.01.1, quay.io/k8scsi/csi-provisioner:v1.5.0, quay.io/k8scsi/csi-attacher:v2.1.0, quay.io/k8scsi/csi-resizer:v0.4.0, quay.io/k8scsi/csi-snapshotter:v2.0.1,
trident-csi-5fc5f:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,
trident-csi-bpdnm:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,
trident-csi-hvk8s:      netapp/trident:20.01.1, quay.io/k8scsi/csi-node-driver-registrar:v1.2.0,
```
What is interesting to notice is that when upgrading Kubernetes, new sidecars are added to CSI Trident:
- Kubernetes 1.16: Volume Expansion (CSI Resizer) was promoted to Beta status (cf https://kubernetes-csi.github.io/docs/volume-expansion.html)
- Kubernetes 1.17: Snapshot & Restore (CSI Snapshotter) was promoted to Beta status (cf https://kubernetes-csi.github.io/docs/snapshot-restore-feature.html)