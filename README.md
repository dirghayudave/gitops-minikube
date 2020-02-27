# gitops-minikube
Gitops Test project which is having application deployed on minikube and flux to automate infrastructure changes from github repo.
In this test case we will be using flux which will pull the latest configuration from the github repo and apply that configurations on specified k8s cluster.

## Pre-requisites 
	Minikube installed on Ubuntu system
	Bash shell access
	github account and 1 repo
	dockerhub account to store docker image

### start minikube cluster 
	minikube start -p gitops --vm-driver=virtualbox

### Install fluxctl in ubuntu bases system where you have installed minikube cluster
	snap install fluxctl --classic

### Create name space for the flux in kubernetes cluster with kubectl
	kubectl create namespace flux

### Export required parameters to configure flux
	export FLUX_FORWARD_NAMESPACE=flux
	export GHUSER="Your git user name"

### Using fluxctl deploy it in k8s cluster, In below specified command it will pull configuration from the /namespaces and /qa from the specified github repo.
	fluxctl install --git-user=${GUSER} --git-email=${GUSER}@users.noreply.github.com --git-url=git@github.com:${GUSER}/gitops-minikube --git-path=namespaces,qa --namespace=flux | kubectl apply -f -

### Rollout deployment of flux.
	kubectl -n flux rollout status deployment/flux

### Generate the identity of 
	fluxctl identity --k8s-fwd-ns flux
Note: Save the identity key into your repo: Settings --> Deploy keys --> Add Deploy key --> Add key.

### Sync flux config and apply
	fluxctl sync

### Check k8s namespaces you can see namespace created by flux for the application deployment.
	kubectl get namespaces

	kubectl get pods --all-namespaces

### Check the list of workload deployed by flux and it's images.
	fluxctl -n [Namespace] list-workloads

	fluxctl -n [Namespace] list-images

### To automate the workload to fetch deployment.
	fluxctl automate --workload=[namespace]:deployment/hello

### Release the workload 
	fluxctl release --workload=[namespace]:deployment/hello --update-image=linuxacademycontent/gitops:hellov1.2
