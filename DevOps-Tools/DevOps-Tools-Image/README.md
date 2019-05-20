This README describes a way to NOT have to use you laptop to install tools etc. Instead we have created a Ubuntu imgage that you will manually install into the kubernetes ionstance you created. This iamge has all the Command Lines - CLI packages preinstalled and does thus no require you to install them on you laptop.

The advantage is that no perculiar setup on your lap top wiull cause problems

Note: When you first install the image we recommend that you perform all the uopdates dewcibed in update section to ensure everything up to date




##Setup
1. Unfortunealy you do need to install kubectl no matter what on your desktop
2. once you have done this 



## Items preinstalled

1. `IBM Kubernetes tools` This installs several packages into the instance
	
	curl -sL https://ibm.biz/idt-installer | bash

1. helm
1. kubectl
1. ibmcloud
1. git






##Logging on to the image in IKS
1. Option 1 - You can use the `Web Terminal (beta)` to do certain commands directly on the IKS pods

1. Option 2 - login from your laptop to IKS. Follow the "Access" tab in the IKS instance dashboard. To change the resourse group use

	ibmcloud login -a https://cloud.ibm.com
	ibmcloud ks region-set us-south
	ibmcloud target -g Education
		Education os the resource group that you created the IKS  instance in
	ibmcloud ks cluster-config devops-cluster





##Update your image

## Individual steps performed

* Install IBM cloud tools

	sudo apt update
	sudo apt-get -y install curl
	curl -sL https://ibm.biz/idt-installer | bash
* Initialize and add Helm repositories

	helm init --client-only
	helm repo add ibm-charts https://icr.io/helm/ibm-charts
	helm repo add iks-charts https://icr.io/helm/iks-charts
	helm repo add ibm-community https://icr.io/helm/ibm-community
	helm repo update
* Install Chart museum
	
	curl -LO https://s3.amazonaws.com/chartmuseum/release/latest/bin/linux/amd64/chartmuseum
	chmod +x ./chartmuseum
	mv ./chartmuseum /usr/local/bin
	chartmuseum --version

* Clone this repository into the image - we will be using many of the scripts on the command line

	git clone https://github.com/mjdavisibm/devops-tools

	
##Option 2 - Using Docker on you laptop to run an OS to communicate with IKS


## Mac OS
1. Install docker GUI from : [Dockerhub](https://hub.docker.com/?overlay=onboarding)
1. Once started open the kitematic application (right hand click on the whale)
1. Once Kitematic opens install the `ubuntu` container
	Select `new+` and search for `ubuntu` and install it - it will automatically run
1. Once started from a command prompt on you laptop log onto the `ubuntu` image

	docker ps | grep ubuntu
		-->>aacf09117b12  ubuntu-upstart:latest "/sbin/init" 20 hours ago  Up 20 hours  0.0.0.0:32768->22/tcp  ubuntu-upstart
	docker exec -it aacf09117b12 /bin/bash
		---> root@aacf09117b12:/#
1. Execute the commands (ONE AT A TIME) in the 'Individual steps performed' section above to prepare your ubuntu image
1. At this point you can perform _all_ the rest of the DevOps-Tools commands within that docker container and  not contaminate you own desktop.