# Prerequisites 

## Virtual or Physical Machines

1. Create 4 virtual or physical ARM64 or AMD64 machines running Debian 12 (bookworm). Please see the table below for specifications of the machines;

|    Name     |     Description    |    CPU     |     RAM    |     Storage    |
|:--------|:--------|:--------|:--------|:--------|
|    Jumpbox     |    Administration host    |    1     |    512MB     |    10GB     |
|    server     |    Kubernetes server     |    1     |   2GB      |      20GB   |
|    node-0     |    Kubernetes worker node     |    1     |    2GB     |     20GB    |
|    node-1     |    Kubernetes worker node     |  1       |    2GB     |     20GB    |

2. Connect to the instance using SSH.
3. Run `cat /etc/os-release` to validate the servers meet the requirement. At the time of documenting this, Debian 12 was on the market place, so I used what was available. See image below:

![alt text](<Kuberntes_project_images/confirm os release of your machine.png>)