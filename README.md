# CKA

---


**Commands**

| Static Pods | Daemon Sets |
|-------------|-------------|
| kubectl drain <nodename>                                | Created by kubeapi server                                     |
| kubectl cordon <nodename>                               | To make the node as unschedulable                             |
| kubectl uncordon <nodename>                             | To make the node back as schedulable                          | 

---

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

git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f deploy/1.8+/

kubectl top node
kubectl top pod

![Alt Text](https://github.com/venkatamamidibathula/CKA/blob/develop/Metrics.png)

---

### ***Application Lifecycle Management ***



To check the status of rollout use the command **kubectl rollout {nameofdeployment}**.

To see the history of rollout **kubectl rollout {nameofdeployment}**.


**Deployment Startegy**

Default deployment strategy is rolling update. There is another strategy which is recreate.

**kubectl apply -f {deploymentdefinition.yaml}**.

**kubectl set image {nameofdeployment} {updated version}** -> **kubectl set image deployment/myapp-deployment \nginx-container=nginx:1.9.1**.


**kubectl describe deployment** can be used to identify if the deployment is rolling or recreate.


**kubectl get replicasets**.

To undo the rollout of deployment we use the command **kubectl rollout undo {nameofdeployment}**


**CMD** parameter in docker works in a way where in the runtime parameters will replace the CMD parameters defined in docker file.

**ENTRYPOINT** parameter in docker works in a way where in the run time parameters get appended to entrypoint command.


> docker file: ubuntusleep
>
> from ubuntu
>
> CMD ["sleep","5"]
>
> docker run ubuntusleep sleep 10
>
> docker file: ubuntusleep 
>
> from ubuntu
>
> ENTRYPOINT ["SLEEP"]
>
> docker run ubuntusleep 5 
>
> The value 5 gets appended to sleep command

Environment variables are defined in three ways:

- **Plain Key value pairs**
- **Config maps**
- **Secret keys**

**Config maps**

The config maps are key value pairs which can store value and we can refer these config maps in pod definition file for parsing environment values

apiVersio: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod


![Alt Text](https://github.com/venkatamamidibathula/CKA/blob/develop/ConfigMaps.png)


**To create a pod definition out a running pod : kubectl get pod <pod-name> --output=yaml > pod-definition.yaml**

**Multi-Container Pods**

Monolithic application is huge and hard to maintain. 

The application needs to be broken down to small micro-services that can be managed easily and scaled up.

In a multi-container pod a webserver and log agent are placed in a single pod sothat the logs agent can collect logs sent by the webserver.


```yaml

apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
    - name: webserver
      image: nginx
      ports:
        - containerPort: 8080 
    - name: logagent
      image: log-agent

```

Let us assume an application hosted on a pod **app** which is in **elastic-stack** namespace and that app outputs logs into **/log/app.log**
To check the logs output **kubectl -n elastic-stack exec -it app -- cat /log/app.log** or **kubectl exec -ti app --namespace=elastic-stack -- cat /log/app.log**


**Init containers**

In a multi container suppose you want a container A to complete the process prioto container B to start we place container A under init containers section.

In the below example we want to ensure the clone is completed first prior to application starting, so we place cloning container under initContainers section.

```yaml

apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ; done;']

```

---

### ***Cluster Maintenance***

Whenver a node goes down the pods inside the node also goes down.
If the node comes back online within 5 minutes the pods will be made available on that node.

Suppose if the node does not come back online the pods will be evicted. The time it takes to for the pods to be come back online is pod eviction timeout.

Default pod eviction timeout is 5 minutes. If a node goes offline master node waits for 5 minutes to check if the node comes back online.


