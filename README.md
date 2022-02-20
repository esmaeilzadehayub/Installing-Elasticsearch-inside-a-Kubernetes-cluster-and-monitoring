# Installing-Elasticsearch-inside-a-Kubernetes-cluster-and-monitoring
There are two solution to install Elasticsearch in Kubernetes.

1. Manifest: you can use manifests or .yml for installing ELK in Kubernetes in the kubernetes-elk-manifests folder.
  - Using RBAC Authorization, 
   A ClusterRole can be used to grant the same permissions as a Role. Because ClusterRoles are cluster-scoped, you can also use them to grant access to:

cluster-scoped resources (like nodes)
non-resource endpoints (like /healthz)
namespaced resources (like Pods), across all namespaces
For example: you can use a ClusterRole to allow a particular user to run 
```sh
kubectl get pods --all-namespaces
```
 ClusterRoleBinding example
To grant permissions across a whole cluster, you can use a ClusterRoleBinding. The following ClusterRoleBinding allows any user in the group "manager" to read secrets in any namespace.
  ```yml
    # RBAC authn and authz
apiVersion: v1
kind: ServiceAccount
metadata:
  name: elasticsearch-logging
  namespace: kube-system
  labels:
    k8s-app: elasticsearch-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: elasticsearch-logging
  labels:
    k8s-app: elasticsearch-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
rules:
- apiGroups:
  - ""
  resources:
  - "services"
  - "namespaces"
  - "endpoints"
  verbs:
  - "get"
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: kube-system
  name: elasticsearch-logging
  labels:
    k8s-app: elasticsearch-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
subjects:
- kind: ServiceAccount
  name: elasticsearch-logging
  namespace: kube-system
  apiGroup: ""
roleRef:
  kind: ClusterRole
  name: elasticsearch-logging
  apiGroup: ""
 ```
2. You can use Helm chart to install ELK stack in elk_helm_chart folder
