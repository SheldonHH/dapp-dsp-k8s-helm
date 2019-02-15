# Deploying a full DSP using K8S

## Getting started
### AWS
https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html

### GCP
https://cloud.google.com/kubernetes-engine/docs/quickstart

## Deployment using helm
### Install helm

Download client from: https://docs.helm.sh/using_helm/#installing-helm
#### Ubuntu
```bash
sudo snap install helm --classic
```

Run:
```bash
helm init
helm repo add liquidapps https://s3-us-west-2.amazonaws.com/liquidapps-helm-charts/
helm update repo

kubectl create serviceaccount --namespace kube-system tiller 
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller 
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'

```

### Clone the DAPP-DSP helm chart

```bash
git clone https://github.com/liquidapps-io/dapp-dsp-k8s-helm.git
cd dapp-dsp-k8s-helm
```
### Edit values.yaml

### Deploy
Restore from snapshot:
```bash
helm install --set eosnode.snapshot=true .
```
Or restore from full backup and replay:
```bash
helm install --set eosnode.snapshot=true --set eosnode.replay=true .
```
Or resume after first restore:
```bash
helm install .
```