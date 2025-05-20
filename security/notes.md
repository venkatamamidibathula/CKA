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

