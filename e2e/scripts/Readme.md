## OpenEBS on AZURE - Deployment using Shell script

The purpose of this user guide is to provide the instructions to set up a Kubernetes cluster on AKS and have OpenEBS running in hyperconverged mode.

### Prerequisites

- An Azure account

Download the script file called oebs_aks.sh.
```
    $ mkdir -p openebs
    $ cd openebs
    $ wget https://raw.githubusercontent.com/openebs/openebs/master/e2e/scripts/oebs_aks.sh
    $ chmod +x oebs_aks.sh
```
#### Updating .profile file:

This script require the AZURE credentials to access AKS services.

- Add path /usr/local/bin to the PATH environment variable.
```
    $ vim ~/.profile
    
    # Add the AKS credentials as environment variables in .profile
    export USERNAME="<username>"
    export PASSWORD="<password>"
    export NODE_COUNT="<node count>"
    export NODE_VM_SIZE="<node vm size>"
    
    # Add /usr/local/bin to PATH
    PATH="$HOME/bin:$HOME/.local/bin:/usr/local/bin:$PATH"
    
    $ source ~/.profile
```
**NOTE:** Node count will be the number of nodes, and Node vm size is the size of the virtual machine. For details of node vm size goto the following URL  https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-sizes-specs

### Create a cluster on AKS

Run the oebs_aks.sh to create a Kubernetes Cluster on AKS and deploy OpenEBS.

The script will first install the following prerequisites:
- Azure CLI
- kubectl
Once the Kubernetes Cluster is created, the script goes ahead and installs the ISCSI packages in the kubelet container of all worker nodes.

Finally the script deploys OpenEBS operator and OpenEBS Storage Classes on the cluster

To verify the resource group run the below command.
```
    devops@Azure:~$ az group list
```
To verify the Created cluster run the following command.
```
    devops@Azure:~$ az aks list -g <resource group name>
```
To verify the successful deployment of OpenEBS run the following command.
```
    devops@Azure:~/openebs/k8s$ kubectl get pods 
    NAMESPACE     NAME                                                            READY     STATUS    RESTARTS   AGE
    default       maya-apiserver-69f9db69-gfh4x                                   1/1       Running   0          17h
    default       openebs-provisioner-77cb47986c-4nghb                            1/1       Running   0          17h
```

