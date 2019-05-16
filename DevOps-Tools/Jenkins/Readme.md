# Installation of Jenkins Container onto a Kubernetes cluster in IBM Cloud
_Written on May 15th 2019 using IBM Kubernestes Service (IKS) with Kubernetes version 1.13.6_1521_

This Readme describes how to install the official Jenkins container  on DockerHub (http:hub.docker.com) or (http://dockerhub.com) into an instance of a *Standard* IBM Kubernetes Service (IKS) Cluster. 

> Note the free instance is not recommended as this only lasts to 30 days.
In addition we use IKS's persistent storage capabilities to store the Jenkins' artifacts.
This ensures that the Jenkins artifacts are not destroyed between pod recreation or crashes.
Free does not allow for this.

It is assumed the reader knows how to create an IKS *Standard* instance.

Once the Kubernetes cluster has been created follow these steps to install the official **Jenkins** container stored on DockerHub.

We will be _deploying_ the DockerHub.com container using a Kubernetes Deployment artifact that will automatically create a Pod and ReplicaSet


##1. Create a Kubernetes namespace for all Jenkins artifacts.

Using IBM Cloud's built in `Web Terminal (beta)` create a `jenkins` namespace by typing
	
	$ kubectl create ns jenkins
##2. Create persistent storage for Jenkins artifacts. 

> 
This will ensure that if the Pod goes down that your Jenkins artifacts will not also be blown away.

Using the Kubernetes Dashboard click the `+Create` button in the top right. We will be pasting YAML text into the `CREATE FROM TEXT INPUT` tab.

	1. Paste the text from the 1_PVC_Setup.yaml file found in in this directory into the text box
	2. Press the upload button
	
##3. Installing the Jenkins container from GitHub

Following the same cutting and pasting model decribed above

	1. Paste the text from the 2_Install_Jenkin.yaml file found in in this directory into the text box
	2. Press the upload button
	
######Couple of points to note
* The official Jenkins DockerHub container is called [jenkins/jenkins](https://hub.docker.com/r/jenkins/jenkins/)
* `/var/jenkins_home` is where all the Jenkins artifacts are stored within the container
* Both the Unix UID and GID for the Jenkins user is set to 1000
* The `InitContainer:` section is executed first and installs [BusyBox](https://hub.docker.com/_/busybox). BusyBox combines tiny versions of many common UNIX utilities into a single small container (1-5 MB). It's fast to download and install. The `command:` section that changes the ownership of `/var/jenkins_home` to the Jenkins user and group. Thus when the main `container:` section executes and tries to write configuration information into the home directory it is actually authorized to do that.

##4. Allowing access to the Jenkins instance so you can Login.
Kubernetes requires  a `Service` object to be created in order for users to access the Jenkins application. 

Again using the `+Create` button in the top right of the Kubernetes Dashboard.

	1. Paste the text from the 3_Allow_Jenkins_Access.yaml file found in in this directory into the text box
	2. Press the upload button

##5. 