
# Set Up the JumpBox

In this lab we will set up one of the four machines to be a jumpbox. This machine will be used to run commands throughout this tutorial.

JumpBox is an administration machine that will be used as a home base when setting up Kubernetes cluster from the ground up. Before we get started we need to install a few command line utilities and clone the Kubernetes The Hard Way git repository, which contains some additional configuration files that will be used to configure various Kubernetes components throughout this tutorial.

##### N.B: Run all commands as root. So login as root user. Run `sudo su`

1. Login to the jumpbox server: `ssh -i <"yourkeypair.pem"> <user@publicDNS url>`.

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

The downloaded binaries will be in either the `downloads-amd64.txt` or `downloads-arm64.txt` file, see below.

![alt text](<Kuberntes_project_images/confirm downloads of binaries.png>)

6. Extract the component binaries from the release archives and organize them under the `downloads` directory.

```
{
  ARCH=$(dpkg --print-architecture)
  mkdir -p downloads/{client,cni-plugins,controller,worker}
  tar -xvf downloads/crictl-v1.32.0-linux-${ARCH}.tar.gz \
    -C downloads/worker/
  tar -xvf downloads/containerd-2.1.0-beta.0-linux-${ARCH}.tar.gz \
    --strip-components 1 \
    -C downloads/worker/
  tar -xvf downloads/cni-plugins-linux-${ARCH}-v1.6.2.tgz \
    -C downloads/cni-plugins/
  tar -xvf downloads/etcd-v3.6.0-rc.3-linux-${ARCH}.tar.gz \
    -C downloads/ \
    --strip-components 1 \
    etcd-v3.6.0-rc.3-linux-${ARCH}/etcdctl \
    etcd-v3.6.0-rc.3-linux-${ARCH}/etcd
  mv downloads/{etcdctl,kubectl} downloads/client/
  mv downloads/{etcd,kube-apiserver,kube-controller-manager,kube-scheduler} \
    downloads/controller/
  mv downloads/{kubelet,kube-proxy} downloads/worker/
  mv downloads/runc.${ARCH} downloads/worker/runc
}
```
7. Delete the compressed file `rm -rf downloads/*gz`

8. Change the permission of the different directories to make the binaries executable.

```
{
  chmod +x downloads/{client,cni-plugins,controller,worker}/*
}
```
The results of 7 and 8 above should have all compressed files deleted and all permissions administered to the directories.

![alt text](<Kuberntes_project_images/grant  all permission to the binaries directories..png>)



#### 9. Install Kubectl -  The official kubernetes command line tool.

This will be installed on the jumpbox machine. In the previous step, we made the binary executable. The file is located in the `downloads/client/kubectl` file.

10. Copy it to the `/usr/local/bin/` directory. Run `{
  cp downloads/client/kubectl /usr/local/bin/
}`. Moving kubectl here, places it in a directory that is typically included in the system's $PATH, making it be able to run 

    Run `kubectl version --client` to validate that kubectl has been installed. You should get an output similar to this:

    ![alt text](<Kuberntes_project_images/Verify kubectl is installed.png>).


At this point jumpbox machine has been configured with all command line tools required to commplete the course.




