# K8S Jenskins At Scale

We will use the helm chart deployment management for this project. 
We will deploy it on minikube engine


### start the minikube & check the stauts of the engine
	$ minikube start 
	$ minikube status


### create a name space for this project 
	$ kubectl create namespace jenkins


### configure helm charte for the deploymant
   	# add the repo & update the helm pkg deploymant manager.
	$ helm repo add jenkinsci https://charts.jenkins.io
	$ helm repo update


### apply the volume file to the deployment 
	$ kubectl apply -f jenkins-volume.yaml

- Minikube configured for hostPath sets the permissions on /data to the root account only.
- Once the volume is created you will need to manually change the permissions to allow the jenkins account to write its data.


### ssh to the minikube container
        $ minikube ssh
	$ sudo chown -R 1000:1000 /data/jenkins-volume


### cheeck that the volume has been created
	$ kubectl get pv



### create service account premission  
	$ kubectl apply -f jenkins-sa.yaml


The jenkins-values.yaml is used as a template to provide values that are necessary for setup.

## NodePort selection 
## Setting the service account  
## Installing Plugins 
## Storage Class 


# Installing the Helm Chart


### define the chart name 
	$ chart=jenkinsci/jenkins


### install jenkins helm chart via file
	$ helm install jenkins -n jenkins -f jenkins-values.yaml $chart

### get the user and pass from the deployment
	$ jsonpath="{.data.jenkins-admin-password}"
	$ secret=$(kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath)
	$ echo $(echo $secret | base64 --decode)

### get the url 
	$ jsonpath="{.spec.ports[0].nodePort}"
	$ NODE_PORT=$(kubectl get -n jenkins -o jsonpath=$jsonpath services jenkins)
	$ jsonpath="{.items[0].status.addresses[0].address}"
	$ NODE_IP=$(kubectl get nodes -n jenkins -o jsonpath=$jsonpath)
	$ echo http://$NODE_IP:$NODE_PORT/login



### check the status of the helm deployment
	$ kubectl get all -n jenkins
	$ kubectl get pods -n jenkins


### port forwording the jenkins pod 
	$ kubectl -n jenkins port-forward <pod_name> 8080:8080

Visit 127.0.0.1:8080/ and log in using admin as the username and the password you retrieved earlier.

 
