# ☸️ Week 5 – Task 3: Deploy AKS via Azure Portal, Access Dashboard & Setup RBAC

## 🎯 Objective

For this task, I decided to go beyond just spinning up a Kubernetes cluster. I wanted to **simulate how teams in organizations set up managed Kubernetes on Azure (AKS), access its dashboard, and configure role-based access for multiple users.**

The goal was to:

* Provision a production-grade AKS cluster using the **Azure Portal**.
* Access the Kubernetes dashboard for cluster visualization and management.
* Create multiple **RBAC roles** and assign them to different users, mimicking real-world scenarios where DevOps, Developers, and QA teams have varying levels of access.

---

## Step 1: Prepare the Environment

Before starting, I made sure that:

* I had an **Active Azure subscription**.
* I was logged into the Azure Portal with an account having **Owner/Contributor** rights.

I also created a dedicated **resource group** for AKS to keep everything organized.

* **Resource Group:** `csi-aks-rg`
* **Region:** `Central India`

![Screenshot 2025-07-05 220646](https://github.com/user-attachments/assets/97640e91-a488-477a-a476-220471d96639)


This keeps things clean and makes it easier to tear down resources later to avoid unnecessary costs.

---

## Step 2: Create AKS Cluster via Azure Portal

### Navigate to AKS

* Went to **Azure Portal → Kubernetes services → + Create → Kubernetes cluster**.

![Screenshot 2025-07-05 220658](https://github.com/user-attachments/assets/e4f51349-51e4-4106-a8a5-00268050ce43)


### Basics Tab

* **Resource Group:** `aks-devops-rg`
* **Cluster Name:** `csi-aks-cluster`
* **Region:** `Central India`
* **Kubernetes version:** Left to default stable.

![Screenshot 2025-07-05 220731](https://github.com/user-attachments/assets/9a53a3be-72a7-49cd-9e0f-fa2629f2e52f)


### Authentication

* Selected **System-assigned Managed Identity** for simplicity.
* Enabled **Role-based access control (RBAC)**.

### Node Pools

* Used the default system node pool:

  * **Node Size:** Standard DS2 v2
  * **Node Count:** 2 (enough for test workloads)

![Screenshot 2025-07-05 220807](https://github.com/user-attachments/assets/172abfba-e74e-42fc-adbb-f75dd7111b0c)


### Review + Create

* Validated and clicked **Create**.

It took around **7-8 mins** to provision the AKS cluster.

![Screenshot 2025-07-05 220827](https://github.com/user-attachments/assets/edee4bfa-2595-4529-a315-afdd487e0daf)


* Successfully Created AKS Cluster

![Screenshot 2025-07-05 220841](https://github.com/user-attachments/assets/839c7a38-265e-46e7-a4e2-3dd8743e30f4)


---

## Step 3: Connect to AKS via Cloud Shell

Once the cluster was ready, I opened the Azure Cloud Shell (Bash) and connected to the AKS cluster using:

```bash
az aks get-credentials --resource-group csi-aks-rg --name csi-aks-cluster
```
This updated my ~/.kube/config file with the cluster context, allowing me to use kubectl locally.

This fetched my `kubeconfig` so I could interact with the cluster using `kubectl`.

To test:

```bash
kubectl get nodes
```

And I saw both nodes listed and in Ready state. So everything was working as expected.

![Screenshot 2025-07-05 220900](https://github.com/user-attachments/assets/5b6cb61b-73c6-4d21-8158-69dc58564c98)



---

## Step 4: Access Kubernetes Dashboard

Since AKS doesn’t deploy the Kubernetes Dashboard by default (for security reasons), I deployed it manually using:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
This set up all the necessary resources, including the kubernetes-dashboard namespace, service, deployment, etc.

![Screenshot 2025-07-05 220917](https://github.com/user-attachments/assets/7a615cb8-258a-4b6d-bef1-2801eab9acb7)


This deployed everything into the kubernetes-dashboard namespace: the service, deployment, role bindings, and more.

```bash
kubectl get all -n kubernetes-dashboard
```

![Screenshot 2025-07-05 220935](https://github.com/user-attachments/assets/db9513c7-251b-4e7e-940f-2f971db8dabf)


### Edit the Dashboard Service (Expose via LoadBalancer)

By default, the dashboard service is of type ClusterIP, which is not accessible externally. I edited it to change the type to LoadBalancer so I could access it securely from a browser.

```bash
kubectl edit svc kubernetes-dashboard -n kubernetes-dashboard
```

In the YAML editor, I changed this `type: ClusterIP` to `type: LoadBalancer`

![Screenshot 2025-07-05 220948](https://github.com/user-attachments/assets/829aa6d8-1d7e-4d02-9132-40b42e03db55)


his tells Kubernetes to provision an external IP.

### Get the LoadBalancer External IP

After updating the service, I ran

```bash
kubectl get svc -n kubernetes-dashboard
```
![Screenshot 2025-07-05 221001](https://github.com/user-attachments/assets/129574d5-a436-4597-bcba-4fa4518a3a19)


Now the Dashboard was accessible securely in Browser

### Create a Token for Login
To log in securely, I needed an authentication token. Since I had already granted cluster-admin privileges to the dashboard's service account using the following:

```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: dashboard-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
```

I simply retrieved the token using:

```bash
kubectl -n kubernetes-dashboard create token kubernetes-dashboard
```

![Screenshot 2025-07-05 221020](https://github.com/user-attachments/assets/a2a1ecdc-c2bd-402b-87c5-09291bf3361e)


This returned a long JWT token which I copied for login.

### Login via Token over HTTPS

Once I had the token and the LoadBalancer IP, I opened in browser

![Screenshot 2025-07-05 221035](https://github.com/user-attachments/assets/c4631693-ef30-40c8-a24b-583e7d58ac72)


## 👥 Step 5: Configure Roles for Multiple Users

To mimic a real company scenario, I created different RBAC roles.


### DevOps - Full Cluster Admin Access

```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: devops-admin
subjects:
- kind: User
  name: devops@example.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
```
This gives devops@example.com full control over the cluster — perfect for operations and infrastructure roles.

![Screenshot 2025-07-05 221051](https://github.com/user-attachments/assets/53a6185b-535a-4392-8739-5679bceb6625)



### Developer - View-Only Access (Default Namespace)

```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: developer-read
  namespace: default
subjects:
- kind: User
  name: developer@example.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: view
  apiGroup: rbac.authorization.k8s.io
```

This role restricts the developer to view-only access in the default namespace, which is a common real-world setup to avoid accidental disruptions.

![Screenshot 2025-07-05 221105](https://github.com/user-attachments/assets/25ddd579-727b-4e27-8551-3048a69ea787)



### QA Team Namespace Role - Read-Only Access to QA Namespace

```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: qa-read
  namespace: qa-environment
subjects:
- kind: User
  name: qa@example.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: view
  apiGroup: rbac.authorization.k8s.io
```

This grants QA users read-only access to the qa-environment namespace — useful for verifying deployments and debugging without having write access.

![Screenshot 2025-07-05 221124](https://github.com/user-attachments/assets/008abaed-ce3d-435d-b207-a98435f20300)


---

## Conclusion

In this task, I successfully provisioned a production-grade AKS cluster on Azure and accessed the Kubernetes Dashboard. I also simulated a real-world scenario by configuring RBAC roles for different users—granting full admin access to DevOps, view-only access to Developers, and read-only access to QA.

---
