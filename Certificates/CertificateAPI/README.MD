The certificate API is used to sign certificate requests requested by users who would like to join the cluster.
Control Manager is responsible for signing and approving certificates

As part of the process. Below are the steps the users perform:

#1: First the user creates a key
openssl genrsa -out jane.key 2048


#2. The user sends the key to cluster administrator to create a certificate signing request 
openssl req -new -key jane.key "/CN=jane" -out jane.csr

#3. Whenver sending a certificate try to convert the certificate info to base64 format
cat jane.csr | base 64 | tr -d "\n"

#4. To see the csr we use the below 
kubectl get csr

#5. To approve certificate 
kubectl certificate approve jane

#6. To get csr informatio
kubectl get csr jane -o yaml

#7. To decocde the certificate 
echo " " | base64 --decode

#8. To approve a csr 
kubectl certificate approve <nameofcsr>
==================================================================================

#9 . To get more information about CSR
kubectl get csr agent-smith -o yaml

#10. To reject a CSR
kubectl certificate deny agent-smith

#11. To Delete a CSR 
kubectl delete csr agent-smith

====================================================================================

Kubectl Config

![Alt text](image.png)

# To see the context 
kubectl config view 

# To switch context 
kubectl config use-context {user}@{cluster}
Example: kubectl config use-context prod-user@production
context is nothing but combination of {user}@{cluster}



#I would like to use the dev-user to access test-cluster-1. Set the current context to the right one so I can do that.
kubectl config --kubeconfig=/root/my-kube-config use-context research


#We don't want to have to specify the kubeconfig file option on each command. Make the my-kube-config file the default kubeconfig.
Replace the contents in the default kubeconfig file with the content from my-kube-config file
The local of default config can be found in ls -a -> .kube -> config

# To know the current context
kubectl config current-context


==============================================================================================





