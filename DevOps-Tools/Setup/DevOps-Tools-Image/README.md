# Installing DevOps-Tools via a containerized instance of Ubuntu
This section describes a way of creating a containerized `ubuntu` image to perform the setup commands described in the parent folders _README.md_. The two options described are

1. **Docker**. Using a Docker `ubuntu` image on your laptop using the [Docker app](https://www.docker.com/get-started). This requires the `docker` CLI to be installed on your laptop.
2. **IKS**. Using the same Docker `ubuntu` image but executing in you DevOps-Tools IKS instance and accessing it using `ibmcloud` CLI. This requires installing the `ibmcloud` CLI on your laptop

## Docker hosting the Ubuntu Image  on you Laptop
The current README only contains a solution for Mac OS. 

##### Setup on Mac OS
1. Install docker GUI from [docker.com](https://hub.docker.com/?overlay=onboarding). Follow the instructions there
2. Start Docker. A whale icon appears at the top of the screen.
3. Open the **Kitematic** application (right hand click on the whale)
4. Select `new+` and search for `ubuntu` 
5. Install the container - it will run automatically

#####Accessing the Docker `ubuntu` Image
From a command prompt on you laptop access the Docker hosted `ubuntu` image using

	docker ps | grep ubuntu
		-->>aacf09117b12  ubuntu-upstart:latest "/sbin/init" 20 hours ago  Up 20 hours  0.0.0.0:32768->22/tcp  ubuntu-upstart
		          |
		           ----
		               |
		               V
	docker exec -it aacf09117b12 /bin/bash
		---> root@aacf09117b12:/#
		
## IKS hosting the ubuntu Image in the DevOps-Tools IKS Cluster
This installs an `ubuntu` image into a pod on the DevOps-Tools IKS cluster. It requires several more steps to get to the `ubuntu` command prompt as you have to first access the DevOps-Tools IKS cluster from your laptop using the `ibmcloud` CLI

#####Setup using IKS

1. Log into IBM Cloud using your account
2. Got the IKS DevOps-Tools cluster
3. Open the **Kubernetes Dashboard**
4. Click the `+Create` button in the top right corner and paste the `ubuntu_pod.yaml` file into the `CREATE FROM TEXT INPUT` tab
5. Press upload.

> Note we use the default namespace for this image

To setup your laptop you need to install (at a minimum) the `ibmcloud` CLI. However, you can follow the instructions in the _Access_ tab on the DevOps-Tools cluster home page which will install `ibmcloud` and several other CLI onto your laptop.

1. To install just `ibmcloud` follow the instructions [here](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud-cli)
2. The command shown on the DevOps-Tools IKS cluster home page type
	
	curl -sL https://ibm.biz/idt-installer | bash

##### Accessing the IKS `ubuntu` Image
1. You can use the `Web Terminal (beta)`, found on the DevOps-Tools IKS cluster home page in the top right, to do certain commands directly on the IKS instance without any setup
2. Follow the commands in the _"Access"_ tab in the `devops-cluster` IKS cluster console. See **Accessing the IKS Cluster** file in the3 top directory

	kubectl exec -it ubuntu -- /bin/bash
