# argocd-gitops
Kubernetes Setup

CLUSTER_NAME=ps-eks-001
eksctl anywhere generate clusterconfig $CLUSTER_NAME \
   --provider vsphere > ps-eks-001.yaml



export EKSA_VSPHERE_USERNAME='administrator@pushingstart.com'
export EKSA_VSPHERE_PASSWORD='Fordmustang1313!!!'

sudo eksctl anywhere create cluster -f eksa-mgmt-cluster.yaml 

kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: standard
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"
provisioner: csi.vsphere.vmware.com
parameters:
  datastoreurl: "ds:///vmfs/volumes/617fb5e6-f8690fcb-336f-b8ca3a6bd82c/"
reclaimPolicy: Delete
allowVolumeExpansion: true


apiVersion: v1
kind: Secret
metadata:
  name: eck-trial-license
  namespace: elastic-system
  labels:
    license.k8s.elastic.co/type: enterprise_trial
  annotations:
    elastic.co/eula: accepted

kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.12.1/manifests/namespace.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 10.5.115.1-10.5.118.254


kubectl get configmap kube-proxy -n kube-system -o yaml | \
sed -e "s/strictARP: false/strictARP: true/" | \
kubectl apply -f - -n kube-system

kubectl create -f https://download.elastic.co/downloads/eck/2.1.0/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/2.1.0/operator.yaml
      
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.12.1/manifests/metallb.yaml

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml



 

export GOVC_URL=vcenter.pushingstart.com
export GOVC_USERNAME='administrator@pushingstart.com'
export GOVC_PASSWORD='Fordmustang1313!!!'
export GOVC_INSECURE=true



kubectl expose deployment kibana-kb --port=5601 --type=LoadBalancer --name=kibana-kb-lb

kubectl expose service elasticsearch-es-http --port=9200 --type=LoadBalancer --name=elasticsearch-es-http-lb

kubectl expose service apm-server-elasticsearch-apm-http --port=8200 --type=LoadBalancer --name=apm-server-elasticsearch-apm-http-lb


kubectl expose service fleet-server-agent-http --port=8220 --type=LoadBalancer --name=jenkins-1649536123-lb

kubectl expose service harbor-1649553719-registry --port=5000 --type=LoadBalancer --name=harbor-1649553719-registry-lb
