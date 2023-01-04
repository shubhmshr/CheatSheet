- List the namespace on the cluster
`kubectl get namespaces`
- List the pods in all namespaces
`kubectl get pods --all-namespaces`
- List the pods in the `kube-public` namespace
`kubectl -n kube-public get pods`
- List ConfigMap objects
`kubectl -n kube-public get configmaps`
- Inspect cluster-info
`kubectl -n kube-public get configmaps`
`kubectl -n kube-public get configmap cluster-info -o yaml`
- kube-node-lease