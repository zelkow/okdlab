__Creating a simple OKD cluster for kubeflow testing__
OKD version 4.x
infra : IRBLAB
---

Sources : 

OKD latest official installation : https://docs.okd.io/latest/architecture/architecture-installation.html

OKD repo : https://github.com/openshift/okd/releases

Tuto : https://itnext.io/guide-installing-an-okd-4-5-cluster-508a2631cbee



__Install Strategy__

I want to install on IBM cloud.

1. Prepare architecture of infra and OKD
2. Prepare naming and scripts
3. Install infrastructure 
4. Install okd
5. provide access to webconsole
6. setup simple nginx and check accesses

__Cluster config__


The cluster is for POC porpuses
The infra should be cheap and can be use to test (but not provides) HA and DR if necessary.
3 master nodes are the minimum
2 worker nodes are the minimum

hostnames  | roles     |os |vCPU   |RAM    |Storage    | internal IP   | Public IP 
-----------|-----------|---|-------|-------|-----------| --------------|------------
ibmfra-vm-sdgo-okd-bootstrap|boostrap|coreOS|4|16|120|  | |
ibmfra-vm-sdgo-okd-master1|boostrap|coreOS|4|16|120|  | |
ibmfra-vm-sdgo-okd-master2|boostrap|coreOS|4|16|120|  | |
ibmfra-vm-sdgo-okd-master3|boostrap|coreOS|4|16|120|  | |
ibmfra-vm-sdgo-okd-worker1|boostrap|coreOS|4|16|120|  | |
ibmfra-vm-sdgo-okd-worker2|boostrap|coreOS|4|16|120|  | |

Fedora Core OS provides auto configuration and updates via ignition.
Fedora is source to RHE and CentOS.

OKD provides scripts to set up infra on AWS, Azure, GCP, RHOSP, VMware but not ... IBMcloud.
Therefor I need to provision the infra my self and provide info to the installer

The advantages are that I can automate separatly the infra provisioning and OKD install.
I can bring up and down the infra to minimise costs. I can add RHE worker nodes.
BUT I need to set up : the VMs, LB, network, DNS and Storage.

The architecture is set up in the 
    install-config.yaml

__Pre-req__

CoreOS
As IBM does not have CoreOS as a base image:
- create storage service 
- configure bucket 
- authorize bucket
- download image type Images must be a qcow2 file type, 100GB or less and cloud-init enabled.
- install plugin aspera to download images


__Installation Procedure__

Configure DHCP or set static IP addresses on each node.

Provision the required load balancers.
> layer 4 LB only (Raw TCP, SSL Passthrougth)
> a stateless LB (without session persistance)

Configure the ports for your machines.

Configure DNS.

Ensure network connectivity. 
Check this for ports to open : https://docs.okd.io/latest/installing/installing_bare_metal/installing-bare-metal.html



