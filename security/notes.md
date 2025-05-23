**Security**

**Authentication**

- Who can access?
- Usernames and tokens?
- Certificates
- RBAC Authorization
- ABAC Authorization


**Securing access to kubeapi-server should be our first line of defence**

**Authentication**

**Admins** , **Developers**, **Service Accounts (bots)**

**kube-apiserver**

RBAC: Role Based Access control

Users are assigned specific roles.

Roles are created and role bindings are created which map roles to users

**developer role**

```yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["create","view","list","get"]
  resourceNames: ["blue","orange"]
- apiGroups: [""]
  resource: ["replicasets"]
  verbs: ["create","view","list","get"]

```
**role binding**

```yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io


```

**Roles** and **Role Bindings** are namespace scoped.
Unless and until specified roles and rolebindings will be associated to default namespace.

**Cluster roles** and **Cluster role bindings** are cluster scoped.
Cluster roles and Cluster role bindings are scoped to a cluster. If you create a cluster role to view pods. When a user is assigned this role he will be able to view pods across the cluster.

To view roles in namespaces

- **kubectl api-resouces --namespaces=true**

To view roles outside namespaces

- **kubectl api-resources --namespaces=false**


**Cluster role**

```yaml


apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-administrator
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["list","get","create","delete"]

```

**Cluster role binding**
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-role-binding
subjects:
- kind: User
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-administrator
  apiGroup: rbac.authorization.k8s.io

```

Example Question:

A new user michelle joined the team. She will be focusing on the nodes in the cluster. Create the required ClusterRoles and ClusterRoleBindings so she gets access to the nodes.


Fist step create cluster role:

- kubectl create clusterrole node-adminrole --verb=* --resource=nodes --dry-run=client -o yaml > nod
erole.yaml

Second step create cluster role binding:

- kubectl create rolebinding michelle-binding --clusterrole=node-adminrole --user=michelle --namespace=default --dry-run=client -o yaml > nodebinding.yaml

Third step

- run kubectl apply -f .

Example Question:


michelle's responsibilities are growing and now she will be responsible for storage as well. Create the required ClusterRoles and ClusterRoleBindings to allow her access to Storage.


Get the API groups and resource names from command kubectl api-resources. Use the given spec:



ClusterRole: storage-admin

Resource: persistentvolumes

Resource: storageclasses

ClusterRoleBinding: michelle-storage-admin

ClusterRoleBinding Subject: michelle

ClusterRoleBinding Role: storage-admin




Solution:

- Create a cluster role
kubectl create clusterrole storage-admin --resource=persistentvolumes,storageclasses --verb=* --dry-run=client -o yaml > storageadmin.yaml

- Create a cluster role binding

kubectl create clusterrolebinding michelle-storage-admin --user=michelle --clusterrole=storage-admin --dry-run=client -o yaml > storageclusterbindadmin.yaml

- Run apply

kubectl apply -f .
