# Task 4

## Rakshith's Kubernetes Cluster

The platform used is AWS, as I have some promotional and student credits balance left. I used that to create this Kubernetes cluster. 

### Step 1: Create Kubernetes Cluster Using eksctl

```
eksctl create cluster -f infrastructure/staging-cluster.yaml
```

**Output:**

```
2022-04-15 23:33:03 [ℹ]  eksctl version 0.74.0
2022-04-15 23:33:03 [ℹ]  using region ap-south-1
2022-04-15 23:33:04 [ℹ]  setting availability zones to [ap-south-1c ap-south-1a ap-south-1b]
2022-04-15 23:33:04 [ℹ]  subnets for ap-south-1c - public:192.168.0.0/19 private:192.168.96.0/19
2022-04-15 23:33:04 [ℹ]  subnets for ap-south-1a - public:192.168.32.0/19 private:192.168.128.0/19
2022-04-15 23:33:04 [ℹ]  subnets for ap-south-1b - public:192.168.64.0/19 private:192.168.160.0/19
2022-04-15 23:33:04 [ℹ]  nodegroup "primary" will use "" [AmazonLinux2/1.21]
2022-04-15 23:33:04 [ℹ]  using SSH public key "/Users/rakshith/.ssh/id_rsa.pub" as "eksctl-rakshith-nodegroup-primary-3c:b9:63:87:ac:7a:56:9c:a0:d4:c5:e5:ac:7e:7b:c3" 
2022-04-15 23:33:04 [ℹ]  using Kubernetes version 1.21
2022-04-15 23:33:04 [ℹ]  creating EKS cluster "rakshith" in "ap-south-1" region with managed nodes
2022-04-15 23:33:04 [ℹ]  1 nodegroup (primary) was included (based on the include/exclude rules)
2022-04-15 23:33:04 [ℹ]  will create a CloudFormation stack for cluster itself and 0 nodegroup stack(s)
2022-04-15 23:33:04 [ℹ]  will create a CloudFormation stack for cluster itself and 1 managed nodegroup stack(s)
2022-04-15 23:33:04 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=ap-south-1 --cluster=rakshith'
2022-04-15 23:33:04 [ℹ]  CloudWatch logging will not be enabled for cluster "rakshith" in "ap-south-1"
2022-04-15 23:33:04 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=ap-south-1 --cluster=rakshith'
2022-04-15 23:33:04 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "rakshith" in "ap-south-1"
2022-04-15 23:33:04 [ℹ]  
2 sequential tasks: { create cluster control plane "rakshith", 
    2 sequential sub-tasks: { 
        wait for control plane to become ready,
        create managed nodegroup "primary",
    } 
}
2022-04-15 23:33:04 [ℹ]  building cluster stack "eksctl-rakshith-cluster"
2022-04-15 23:33:05 [ℹ]  deploying stack "eksctl-rakshith-cluster"
2022-04-15 23:45:14 [ℹ]  building managed nodegroup stack "eksctl-rakshith-nodegroup-primary"
2022-04-15 23:45:15 [ℹ]  deploying stack "eksctl-rakshith-nodegroup-primary"
2022-04-15 23:48:32 [ℹ]  waiting for the control plane availability...
2022-04-15 23:48:32 [✔]  saved kubeconfig as "/Users/rakshith/.kube/config"
2022-04-15 23:48:32 [ℹ]  no tasks
2022-04-15 23:48:32 [✔]  all EKS cluster resources for "rakshith" have been created
2022-04-15 23:48:32 [ℹ]  nodegroup "primary" has 3 node(s)
2022-04-15 23:48:32 [ℹ]  node "ip-192-168-35-187.ap-south-1.compute.internal" is ready
2022-04-15 23:48:32 [ℹ]  node "ip-192-168-87-166.ap-south-1.compute.internal" is ready
2022-04-15 23:48:32 [ℹ]  node "ip-192-168-9-86.ap-south-1.compute.internal" is ready
2022-04-15 23:48:32 [ℹ]  waiting for at least 3 node(s) to become ready in "primary"
2022-04-15 23:48:32 [ℹ]  nodegroup "primary" has 3 node(s)
2022-04-15 23:48:32 [ℹ]  node "ip-192-168-35-187.ap-south-1.compute.internal" is ready
2022-04-15 23:48:32 [ℹ]  node "ip-192-168-87-166.ap-south-1.compute.internal" is ready
2022-04-15 23:48:32 [ℹ]  node "ip-192-168-9-86.ap-south-1.compute.internal" is ready
2022-04-15 23:48:33 [ℹ]  kubectl command should work with "/Users/rakshith/.kube/config", try 'kubectl get nodes'
2022-04-15 23:48:33 [✔]  EKS cluster "rakshith" in "ap-south-1" region is ready
```

It took around 15 minutes to spin up a Kubernetes cluster in AWS. I have used *t3.small* instances for creating this cluster.

### Step 2: Deploy Kubernetes Dashboard for Monitoring

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml
```

**Output:**
```
namespace/kubernetes-dashboard created
serviceaccount/kubernetes-dashboard created
service/kubernetes-dashboard created
secret/kubernetes-dashboard-certs created
secret/kubernetes-dashboard-csrf created
secret/kubernetes-dashboard-key-holder created
configmap/kubernetes-dashboard-settings created
role.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
deployment.apps/kubernetes-dashboard created
service/dashboard-metrics-scraper created
deployment.apps/dashboard-metrics-scraper created
```

After deploying the Kubernetes dashboard, 
we will connect to it using `kube proxy` command

Kubectl will make Dashboard available at [http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/).

The next step is we need a user token to access the dashboard, The ServiceAccount and ClusterRoleBinding manifests are defined in *admin-user.yaml* file, after applying these manifest , we will run the following command to retrieve the token.

```
kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}"
```

The token would look something like this.
```
eyJhbGciOiJSUzI1NiIsImtpZCI6Ikk4eFNqRXdPYTZxVnctMi1zR1JTM2owQ1ZoSnVJWkRkeVIwcFNrcmlxU0EifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLTh0ZGI3Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJhOGQxMmEwNC1lMjExLTRkNDctYWU1My1kYmU3YTM4NzJlMTgiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4tdXNlciJ9.ED4kGx1ikdD7V8yVDshPHtSXP2dbXTW3Ih24MiO3BiQorXSabFC-IiiQHCeIk5jfNw28WZg6sVVW9w_Kx2mkcVCQRdrp2tGxv901TVmpvNWkRkFsIFVCAI7ISPE50rSLBaPEG-AZPQ1lK_WxHV57l1UMusX3hdg-YUn1EWbQtfgxWqiUfWc1JeEoBUM7_xKaMxeDXhDOmzJIKJ2gQvXEoEWKzFvfYoU4-y6ywzzdHYPZOWenn3yPD_cioZd_Ff7Mg_ktE_RoeFU9kGZ7JeVIUz7qdCqbo6YyMXQmeJNsdf0jf3yn8AVrQ3tK5I-cpWuPhcKZByxHrL7-dBW_wWaLGw
```
Now copy the token and paste it into the `Enter token` field on the login screen.

![enter image description here](https://raw.githubusercontent.com/kubernetes/dashboard/master/docs/images/signin.png)
Click the `Sign in` button and that's it. You are now logged in as an admin.

![enter image description here](https://raw.githubusercontent.com/kubernetes/dashboard/master/docs/images/overview.png)

### Step 3: Deploy Application Persistance Service - MongoDB in Kubernetes Cluster

