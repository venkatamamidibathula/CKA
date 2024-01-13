# CKA

### ***Scheduling***

----
#### ****Static Pods****

Static pods are used whenver there is no kube api server or master.

The path where the pod definition files need to be kept is shown below.

**Path** = **/etc/kubernetes/manifests**

We can only create PODS this way not replicasets, deployments.

In **kubelet.service** file there is a parameter where you specify the value.

**--pod-manifest-path** = /etc/kubernetes/manfiests \\ or **--config** =kubeconfig.yaml\\ **--kubeconfig** = /var/lib/kubelet/kubeconfig \\ where inside the yaml file you mention. 


> kubeconfig.yaml
>
>**staticPodPath** : /etc/kubernetes/manifests

**Static pods are not dependent on control plane we can use static pods to deploy control plane components as pods on cluster**


**Differences between static pods and Daemon Sets**
| Static Pods | Daemon Sets |
|-------------|-------------|
| Created by Kubelet                                 | Created by kubeapi server                                     |
| Best suited for deploying control plane components | Best suited for deploying monitoring agents, log aggregations |
| Ignored by kube-scheduler                          | Ignored by kube-scheduler                                     | 


---

### ***Logging and Monitoring***

Kubernetes previously had heapster that is used for logging and monitoring but its deprecated.

Metrics server is monitoring service available currently on kubernetes that is in-memory. We can only view current metrics but historical  data will not be available for that we can make use of propreitary tools like Datadog, Prometheus etc.

kubelet also contains a sub-component called **Cadvisor** which is responsible for retrieving perforamnce metrics from PODS and exposing them through the kubelet api for metrics to be available for metrics server.

**Metrics server**= https://github.com/kubernetes-incubator/metrics-server.git

kubectl create -f deploy/1.8+/

kubectl top node
kubectl top pod

![Alt Text](https://github.com/venkatamamidibathula/CKA/blob/develop/Metrics.png)
