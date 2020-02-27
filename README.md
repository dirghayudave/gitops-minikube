# gitops-minikube
Gitops Test project which is having application deployed on minikube and flux to automate infrastructure changes from github repo.

## Pre-requisites 
	Minikube installed on Linux server
	Bash shell access
	github account and 1 repo
	dockerhub account to store docker image



### start minikube cluster 

minikube start -p gitops --vm-driver=virtualbox

