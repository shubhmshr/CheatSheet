- Everything is defined via `YAML` or `JSON`
- `kubectl` reads the `YAML`, connects to `K8 API server` and transfers the `YAML` definition to be stored.
- K8 then takes the definition of the `Pods` that are on API server and makes sure they are up and running somewhere.
- It requires the nodes in the cluster to respond to events that are constantly occurring and updates that state in their Node objects via `kubelet` communicating to the API server.
- It also requires that `OCI images` are present and accessible to the nodes in K8 cluster.
- K8 is `eventually consistent system` i.e. we can continually request changes to the overall state space of all apps in our cluster and lets the underlying K8 platform figure out the logistics of `how` these apps are set in motion over time.

### Kubernetes cluster

![[Pasted image 20221230222028.png]]

- 