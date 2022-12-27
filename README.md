# CROSSPLANE
Crossplane is a tool which works well in spinning up managed services in AWS. Imagine a scenario, you are using a CICD tool for deploying the applications on a kubernetes cluster. You are also managing the infrastructure components seperately using an IaaC tool like Terraform. What if you want to manage containerized applications running on k8s cluster and infrastructure from a single source of truth.
## Scenario
Infrastructure to be setup in AWS cloud  
EKS cluster (v-1.22+)  
Crossplane deployed in EKS cluster  
IAM role to spin up managed resources in AWS  

## Install crossplane in EKS cluster
1. Create a namespace for crossplane
```bash
kubectl create namespace crossplane
```
2. Add the helm repository and update it
```bash
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
```
3. Install/Deploy crossplane
```bash
   helm install crossplane --namespace crossplane crossplane-stable/crossplane
```
4. Get all the resources that got created
```bash
kubectl get all -n crossplane
```
Crossplane will be up and running.

## Install provider config and controller config for Crossplane
1. Install controller config and provider
```bash
kubectl apply -f controller-config.yaml
kubectl apply -f provider.yaml
```
Once you apply this, all the CRDs will be pulled from a public repository.
2. Check the crds available
```bash
kubectl get crds -A
```
3. Install the provider config
   Now we will apply the provider-config to provision new resources.
```bash
kubectl apply -f provider-config.yaml
```

## Provision managed resources in AWS
If you expand the aws directory in this repository, you will see few example yamls to spin up the following aws resources:
1. s3 bucket
2. vpc
3. subnet
4. security-group
5. route-table