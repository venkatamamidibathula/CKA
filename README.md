# CKA

### ***Scheduling***

----
#### ****Static Pods****

Static pods are used whenver there is no kube api server or master.

The path where the pod definition files need to be kept is shown below.

**Path** = **/etc/kubernetes/manifests**

We can only create PODS this way not replicasets, deployments.

In **kubelet.service** file there is a parameter where you specify the value.

**--pod-manifest-path** = /etc/kubernetes/manfiests \\ or **--kubeconfig** = /var/lib/kubelet/kubeconfig \\ where inside the yaml file you mention. 


> kubeconfig.yaml
>
>**staticPodPath** : /etc/kubernetes/manifests


