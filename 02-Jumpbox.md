
# Set Up the JumpBox

In this lab we will set up one of the four machines to be a jumpbox. This machine will be used to run commands throughout this tutorial.

JumpBox is an administration machine that will be used as a home base when setting up Kubernetes cluster from the ground up. Before we get started we need to install a few command line utilities and clone the Kubernetes The Hard Way git repository, which contains some additional configuration files that will be used to configure various Kubernetes components throughout this tutorial.

N.B: Run all commands as root

1. Login to the jumpbox server: `ssh -i "k8skeypair.pem" <user@publicDNS url>`.

2. Install command line Utilities

 ``` 
 {
  apt-get update
  apt-get -y install wget curl vim openssl git
}
```



3. Sync the github repository, It contains configuration files and templates that will be used build your Kubernetes cluster from the ground up. Clone the *Kubernetes The Hard Way git repository* using the `git` command:


```
git clone --depth 1 \https://github.com/kelseyhightower/kubernetes-the-hard-way.git 
```

4. Change into the kubernetes-the-hard-way directory; `cd kubernetes-the-hard-way`. This will be the working directory.

5. Next, download binaries for the different Kubernetes components. The binaries will be stored in the `downloads` directory on the `jumpbox`, this will help avoid downloading the binaries multiple times for each machine in the Kubernetes cluster. Run the command to download;

```
wget -q --show-progress \
  --https-only \
  --timestamping \
  -P downloads \
  -i downloads-$(dpkg --print-architecture).txt
```

Check 
