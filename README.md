# fluend_CloudWatch

##################### Create AWS EKS clsuster ################################################################################
## Create EKS cluster
eksctl create cluster --name my-eks --node-type t2.large --nodes 1 --nodes-min 1 --nodes-max 2 --region us-east-1 --zones=us-east-1a,us-east-1b,us-east-1c

## Get EKS Cluster service
eksctl get cluster --name my-eks --region us-east-1

## Update Kubeconfig 
aws eks update-kubeconfig --name my-eks

## Get EKS Pod data.
kubectl get pods --all-namespaces

## Delete EKS cluster
eksctl delete cluster --name my-eks --region us-east-1

###############################################################################################################################
##################### Commands to follows #####################################################################################
1.  Create your IAM OIDC Identity Provider for your cluster

eksctl utils associate-iam-oidc-provider --cluster my-eks --approve

2. Create the service account for cloudwatch logs

eksctl create iamserviceaccount --name fluentd --namespace kube-system --cluster my-eks --attach-policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy --approve --override-existing-serviceaccounts

3. Create a Cluster Role, and Cluster RoleBinding
kubectl apply -f .\ClusterRole_RoleBinding.yaml

4. Create Kubernetes ConfigMap object for the Fluentd configuration
kubectl apply -f .\K8S_ConfigMap.yaml

5. Script to run the Fluentd container as a DaemonSet object.
kubectl apply -f .\Config_Daemonset.yaml

6. See if the DaemonSet is running or not.
kubectl get ds -n kube-system

7. Deploy samle pods to push logs.
kubectl apply -f .\SampleApp.yaml

############################################################################################################################################
