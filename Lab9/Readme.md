# MySQL InnoDB Cluster on Kubernetes (minikube)
In this lab, we will use minikube to deploy MySQL InnoDB cluster. THe following guide is for **Windows** but the steps are similar on other platforms such as Linux or MacOS
## Install Kubernetes
### Install kubectl
On Windows, install kubectl using the following guide:
https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-windows
Essentially, just download the **kubetcl** to your Windows
### Install Virtualbox
Install Oracle Virtualbox on your Windows by downloading Virtualbox from https://www.virtualbox.org/wiki/Downloads
### Install minikube
Install minikube from https://kubernetes.io/docs/tasks/tools/install-minikube/
You can either use the installer to download the latest minikube binaries or download the binaries directly. Rename the downloaded minikube binaries to **minikube.exe**
## Start minikube
Once you have downloaded the minikube, start up the minikube. This will create a minikube VM in Virtualbox
```
minikube start --cpus 4 --memory 4096
```
## Install MySQL Docker
Download the latest MySQL EE Docker image from support.oracle.com (with a valid Oracle support ID)

![Download](img/K1.png)
Search for the MySQL Docker Image
![Search](img/K2.png)

Once you have downloaded the docker image, upload the image to minikube VM.
Find out the minikube IP address, the minikube configuration is kept in your user directory
```
notepad \\users\\your_name\\.minikube\\machines\\minikube\\config.json
```
```
"Driver": {
        "IPAddress": "192.168.99.103",
```
You need to specify the IP address, private key (id_rsa) and user name **docker** to upload the doker image to the minikube VM
![Upload](img/K3.png)
Use Winscp to transfer the downloaded MySQL docker image to minikube VM
![Upload](img/K4.png)

Once you have uploaded the VM, you need to import the MySQL container
```
ssh -i $(minikube ssh-key) docker@$(minikube ip) -- unzip p30754936_580_Linux-x86-64.zip
ssh -i $(minikube ssh-key) docker@$(minikube ip) -- docker load -i mysql-enterprise-server-8.0.19.tar
ssh -i $(minikube ssh-key) docker@$(minikube ip) -- docker images
```


