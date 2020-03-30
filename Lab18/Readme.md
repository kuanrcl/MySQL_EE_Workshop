# Deploying MySQL in Openshift (using minishift)
Openshift platform is fast becoming the most widely used microservices platform for enterprise application platform. 
In this lab, we will learn how to deploy InnoDB cluster to Openshift platform

## Install minishift 
Download and install minishift from https://github.com/minishift/minishift/releases. 
Once downloaded, run minishft
```
minishift start --cpus 4 --memory 11962
```
What happens is that minishift will check the prerequisites such as Virtualbox installed, etc, as well as to download 
the minishift ISO file from github and install it to Virtualbox.
```
minishift start --cpus 4 --memory 11962
-- Starting profile 'minishift'
-- Check if deprecated options are used ... OK
-- Checking if https://github.com is reachable ... OK
-- Checking if requested OpenShift version 'v3.11.0' is valid ... OK
-- Checking if requested OpenShift version 'v3.11.0' is supported ... OK
-- Checking if requested hypervisor 'virtualbox' is supported on this platform ... OK
-- Checking if VirtualBox is installed ... OK
-- Checking the ISO URL ... OK
-- Checking if provided oc flags are supported ... OK
-- Starting the OpenShift cluster using 'virtualbox' hypervisor ...
-- Starting Minishift VM ............................................... OK
-- Checking for IP address ... OK
-- Checking for nameservers ... OK
-- Checking if external host is reachable from the Minishift VM ...
   Pinging 8.8.8.8 ... OK
-- Checking HTTP connectivity from the VM ...
   Retrieving http://minishift.io/index.html ... OK
-- Checking if persistent storage volume is mounted ... OK
-- Checking available disk space ... 20% used OK
-- Writing current configuration for static assignment of IP address ... OK
-- OpenShift cluster will be configured with ... Version: v3.11.0
-- Copying oc binary from the OpenShift container image to VM ... OK

