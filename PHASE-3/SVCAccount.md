
Create ServiceAccount with YAML

Here’s a simple YAML file to create a ServiceAccount:

vi svc.yaml


command 

 kubectl create ns webapps 
 
 kubectl apply -f svc.yaml
 
========================================

### Creating Service Account


```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
```

command -
create webapps SVC account 
- kubectl create ns webapps
- kubectl apply -f svc.yaml


### Create Role 

file name role.yaml
command - 
- vi role.yaml
- kubectl apply -f role.yaml

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapps
rules:
  - apiGroups:
        - ""
        - apps
        - autoscaling
        - batch
        - extensions
        - policy
        - rbac.authorization.k8s.io
    resources:
      - pods
      - secrets
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - pods
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```



### Bind the role to service account

command -
- vi bind.yaml
- kubectl apply -f bind.yaml


```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-rolebinding
  namespace: webapps 
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role 
subjects:
- namespace: webapps 
  kind: ServiceAccount
  name: jenkins 
```

### Generate token using service account in the namespace

[Create Token](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/#:~:text=To%20create%20a%20non%2Dexpiring,with%20that%20generated%20token%20data.)

#Create - for Createing secrets for jenkins connectivity 

command -
- vi sec.yaml
- kubectl apply -f sec.yaml -n webapps
- kubectl describe secret mysecretname -n webapps
- got output like this 
```yaml
  Name:         mysecretname
Namespace:    webapps
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: jenkins
              kubernetes.io/service-account.uid: b2fca22b-dddd-488b-aa3b-d81df4735390

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1107 bytes
namespace:  7 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IkFIX2V2VHl4NTE4SFBuTTh5aFQ2RjZGWlJ3U0xvS0I0ZWdxNjVjNUE5YzQifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJ3ZWJhcHBzIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6Im15c2VjcmV0bmFtZSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJqZW5raW5zIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiYjJmY2EyMmItZGRkZC00ODhiLWFhM2ItZDgxZGY0NzM1MzkwIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OndlYmFwcHM6amVua2lucyJ9.l2594F6ZDeKdRNTI6D0p4FSynm77r205297ImAsz6dI9moXVnrsvg2ujmFES-Rs-Zh4Y8vfJevNeWpP2r6zl15j4qxIXsxKEJe9fMIFTUTLjGAiB5f2PmEddAT3fuV6kRz0O92f01X6RcgRDUHRrSS6fnqNe9ZdPDdGDR2-dRN2YkciXpK6HGXiOaXM7D4TSKUd9l295DlzJz5N_cdZZST9psjBaZ-COJ-d35omj7feM2XxxO9wdtYniQuXGUaX0s_mjR0DWXE_BWUQfK4XPTcTHcfLX7BHqsZUvNPNmiC8QmDPypgCLa_9kdpA9-96LVkNX5TjO3tFCVfoyh2NDNg
```

```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: jenkins

```
