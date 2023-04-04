# kubernetes-kind
A collection of Kubernetes orchestrations done using my local KinD cluster.

Install KinD using by going to the documentation page and following the procedure to install KinD locally. [Install KinD locally](https://kind.sigs.k8s.io/).

In this particular config, the CNI (Container Network Interface) will not be the default *kindnet* but will be *Cilium* from the Cilium project. 

The procedure to change the default network to this is detailed on their page here, [Installing Cilium on KinD](https://docs.cilium.io/en/stable/installation/kind/).
*NB* The connectivity test may fail when in the above process but it is nothing to fear.

Install MetalLB to be used as the load-balancer in the KinD cluster. Use the KinD docs way to get the best one for the KinD cluster. [Installing MetalLB on KinD cluster](https://kind.sigs.k8s.io/docs/user/loadbalancer/).

You are now ready to start using KinD on the local machine.

Deploy the KinD cluster with a name and referencing the *kind-config.yaml* file.
```Bash
kind create cluster --name mycluster --config kind-config.yaml
```

Here are a couple of commands to use in KinD:
```Bash
Check kind cli:
$ which kind, kind version

List all kind clusters	
$ kind get clusters

Get kubeconfig for a given cluster
$ kind get kubeconfig-path --name $cluster-name

List all nodes for a given cluster
$ kind get nodes --name $cluster_name

Create kind cluster
$ kind create cluster --name clusterapi

Create cluster with different version of k8s
$ kind create cluster --image=kindest/node:v1.15.0@sha256:XXX

Delete kind cluster
$ kind delete cluster --name clusterapi

Load docker image from host to nodes
$ kind load docker-image debian:9


Kind config files are located in the below path
Conf files: 
$ /etc/kubernetes
Services start conf files: 
$ /etc/kubernetes/manifests
kube api server conf file
$ /etc/kubernetes/manifests/kube-apiserver.yaml
```