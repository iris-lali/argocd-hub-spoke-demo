## Argo CD Setup

## Before Installing argocd on  hub-cluster
  
  - make sure you are using the rigth config context as there are three clusters
  - Run: 

```bash
  kubectl config get-contexts
  kubectl config use-context <your-cluster-context>
```

## Install argocd on hub-cluster

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
## Verify namespace is created

```bash
kubectl get ns
```

## Run Argo CD in HTTP Mode(Insecure)

https://github.com/argoproj/argo-cd/blob/54f1572d46d8d611018f4854cf2f24a24a3ac088/docs/operator-manual/argocd-cmd-params-cm.yaml#L82

```bash
kubectl get cm -n argocd
kubectl edit cm argocd-cmd-params-cm -n argocd
```

## Expose Argo CD Server Service in NodePort Mode

```bash
kubectl edit svc argocd-server -n argocd
```
and change the type to NodePort from ClusterIP

## Log into AWS Console

   - Select one of the nodes for hub-cluter
   - Edit the security group (SG) of the node to allow inbound traffic to nodeport port
   - Copy the public IP address of the node and paste it in your browser followed by :<<nodeport>>. EX: 10.x.x.x:31000

## Login to argocd UI

   - Username: admin
   - password: Run the below command to get the password

```bash
kubectl get secret -n argocd # Will show argocd-initial-admin-secret 
kubectl edit secret argocd-initial-admin-secret -n argocd # copy the password to be decoded
echo <<PAWSSORD>> | base64 --decode # Decode the password
```
## Login to argocd using the CLI

```bash
argocd login SERVER #EX: argocd login 10.x.x.x:31000
```
  - Provide the username and the password

## Add spoke clusters to argocd
   
```bash
argocd cluster add << context of the spoker-cluster to be added>> --server <<your-server>>
```

## Create your first application using the UI on the spoke-cluster-1

- Click on Create application
- Provide the application name ex: my first-app
- Project name: default
- Sync Policy: automatic
- Source: provide your github repo url where you have your K8s manifest files
- Path: path to your manifest or the folder of your manifest
- Destination: Select your spoke-cluster1
- NameSpace: provide your namespace; ex: default
- And click on create.

## Create your first application using the UI on the spoke-cluster-2

  - Repeat the same process for spoke-cluster-1 and select spoke-cluster-2 for destination cluster

## Verify Your deployment

   - Switch context to the spoke-cluster-1 and run

```bash
kubectl get all


