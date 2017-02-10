# Contiv Installation for Docker Swarm & Kubernetes 1.4+
Install Contiv on your Docker Swarm or Kubernetes cluster.
## Docker Swarm Installation
### Prerequisites
* CentOS 7.x operating system
* Docker installed on the host where you are running the installer.
* Install a Docker Swarm cluster or use the Contiv installer to install the scheduler stack after installing Docker on a node external to the cluster where the scheduler stack is being installed.

### Contiv Installation

The Contiv Docker Swarm installer is launched from a host external to the cluster.  All the nodes must be accessible to the Contiv Ansible-based installer host through SSH.
![installer](installer.png)
* Download the installer bundle: <br>`curl -L -O https://github.com/contiv/install/releases/download/$VERSION/contiv-$VERSION.tgz`<br>
If your access to the Internet is limited or slow and you want to download the full Contiv install, choose <br>
`contiv-full-$VERSION.tgz`<br>
Note: The full image contains only Contiv components. Installing Docker Swarm will need Internet connectivity.
* Extract the install bundle <br>`tar xf contiv-$VERSION.tgz`.  
* Change directories to the extracted folder `cd contiv-$VERSION`
* To install Contiv with Docker Swarm:
   `./install/ansible/install_swarm.sh -f cfg.yml -e <ssh key> -u <username> -i`
* To install Contiv without the scheduler stack, run the command:<br>
   `./install/ansible/install_swarm.sh -f <host config file> -n <contiv master> -e <ansible ssh key> -a <additional ansible options>` 
* To install Contiv with a scheduler stack, run the command:
   `./install/ansible/install_swarm.sh -f <host config file> -n <contiv master> -e <ansible ssh key> -a <additional ansible options> -i` to install Contiv with the scheduler stack.
* To specify a user to user for the Ansible ssh login, specify "-u <username>" as the additional ansible option.
* Example host config files are available as `install/ansible/cfg.yml` and if you are using ACI,  `install/ansible/aci_cfg.yml`.
* For additional install options, run <br> `./install/ansible/install_swarm.sh -h`.

### Removing Contiv

If you need to remove Contiv from Docker Swarm and return to your original state, you can uninstall Contiv with the following commands:
* To uninstall Contiv and Docker Swarm:<br>
`./install/ansible/uninstall_swarm.sh -f cfg.yml -e <ssh key> -u <username> -i`
* To uninstall Contiv and Docker Swarm with ACI support:<br>
`./install/ansible/uninstall_swarm.sh -f aci_cfg.yml -e <ssh key> -u <username> -i -m aci`
* To uninstall Contiv and not Docker Swarm:<br>
`./install/ansible/uninstall_swarm.sh -f cfg.yml -e <ssh key> -u <username>`
* Note: Adding the `-r` flag, will cleanup any Contiv state.

## Kubernetes 1.4 Installation

### Prerequisites

* CentOS 7.x operating system
* [Install Kubernetes 1.4 or higher](http://kubernetes.io/docs/getting-started-guides/kubeadm/)
* Because the Contiv service-cidr is currently 10.254.0.0/16, the `kubeadm init` step needs to be called with the `--service-cidr 10.254.0.0/16` parameter.
* Replace step (3/4) in the kubeadm install guide with the following Contiv Installation Instructions. Contiv installation can be done after completing step (4/4).

### Contiv Installation
* Use curl to get the installer bundle: <br>`curl -L -O https://github.com/contiv/install/releases/download/$VERSION/contiv-$VERSION.tgz`
* Extract the install bundle <br>`tar xvzf contiv-$VERSION.tgz`. 
* Run `sudo ./install/k8s/install.sh -n $CONTIV_MASTER`
  where `$CONTIV_MASTER` is the Contiv proxy IP.
* To see additional install options, run <br> `./install/ansible/install.sh`.

### Removing Contiv
If you need to remove Contiv, and get back to your original state, run:
`sudo ./install/k8s/uninstall.sh`