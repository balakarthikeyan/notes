Kubernetes is an open-source container orchestration engine for automating deployment, scaling, and management of containerized applications.

What Is Kubernetes?
Kubernetes is popularly known as K8s

K8s is a production-grade open-source container orchestration tool developed by Google to help you manage the containerized/dockerized applications supporting multiple deployment environments like On-premise, cloud, or virtual machines.

k8s automates the deployment of your containerised images and helps the same to scale horizontally to support high level of application availability


kubectl tool can be used debug, analyze Pods, Services in the Kubernetes cluster.

Few useful commands

kubectl get pods
kubectl describe pods
kubectl logs
kubectl exec -ti <pod-name> — bash

Basic Commands of Terraform:

1. Terraform Init → the terraform Init command is used to initialise the working directory containing Terraform configuration file. It is safe to run that command while multiple times.

The command will never delete your exiting configuration or state. During Init , The route configuration directly is consulted for backend configuration and the backend is initialised
using the given configuration setting.

2.Terraform Validate → Basically it is checked the .tf(extension of terraform file)file you created and the code inside in it is correct or not and the syntax of the code have not a single error in it.

3.Terraform Plan → It gives the list of your code that need to be implemented in steps or in what steps you are infrastructure need to design.

4.Terraform Apply → It runs your .TF file and design your infrastructure according to the step mentioned in a file.

5.Terraform Destroy → The Terraform Destroy command is used to destroy the Terraform managed infrastructure.