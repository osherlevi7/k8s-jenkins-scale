# Deployment using YAML deployment file

* setting up the deployment file to : 
	- kind of deployment yaml
	- single replica
	- jenkinscontainer image version to install
	- port exposes {container:80, pod:8080}
	- mounting the volumes


* create a namespace for the deployment 
	$ kubectl create namespace jenkins


* list the namespace 
	$ kubectl get namespaces

* apply the deployment via file 
	$ kubectl create -f jenkins-deployment.yaml -n jenkins

# Grant access to Jenkins service


* create service of type NodePort to beable to access this deployment
	$ kubectl create -f jenkins-service.yaml -n jenkins

* check the service status and IP assingment
	$ kubectl get services -n jenkins

* collect the minikube ip 
	$ minikube ip
* collect the name of the pod that has been created 
	$ kubectl get pods -n jenkins
* display the logs from jenkins
	$ kubectl logs <pod_name> -n jenkins

