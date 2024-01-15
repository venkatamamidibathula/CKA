1. What is the command to drain a node in kubernetes cluster?
   - [ ] kubectl drain node {nodename}
   - [x] kubectl drain {nodename}

2. What is the command to make a node uschedulable?
   - [ ] kubectl cordon node {nodename}
   - [x] kubectl cordon {nodename}

3. What is the command to make a node schedulable after making it unscheduable?
   - [ ] kubectl cordon node {nodename}
   - [x] kubectl uncordon {nodename}


4. What is the pod eviction timeout?
   - [ ] The time taken for pods to getting into running state
   - [x] The time the master node waits to consider the node dead when node goes offline.


5. What is the default pod eviction timeout?
   - [ ] 10 minutes
   - [x] 5 minutes


6. Where is the value for pod evition timeout set?
   - [ ] kubectl-master 
   - [x] kube-controller-manager --pod-eviction-timeout=5m0s
