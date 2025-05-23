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
