# ocp-3.11-prepare-hosts
For me to use in aws for preparing ec2 instances as ocp nodes

- Nodes will be using centos 7 amis
- inventory is static at the moment. When using, update with list of ec2 instances you will
  be using as ocp nodes
- Expects the private key /home/ec2-user/.ssh/aws-jase.pem to exist on the control node
  and this key can be used to access all the nodes
- Docker storage volume will be configured to use overlay2, using the {{ docker_storage_block_device }} 
  inventory variable to identify the available device for docker to use as storage
- "inventory" file is used with prepare_hosts.yml playbook
- "install_inventory" file is used with the okd/ocp prerequisites.yml and deploy_cluster.yml

# General steps
1. Create ansible cn ec2 instance
	  $ sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	  $ sudo yum -y install ansible git
	  
	  Upload aws-jase.pem to the ansible control node to /home/ec2-user/.ssh/

	  *Assumptions: all instances will have internet access

	  $ git clone https://github.com/bkthong/ocp-3.11-prepare-hosts/
	  $ git clone https://github.com/openshift/openshift-ansible
	  $ cd openshift-ansible
	  $ git checkout release-3.11

2. Create the ec2 instances using centos 7 ami with additiobal ebs block device
   that will be used for docker storage. I used 20Gb for root and 20GB for the
   additional block device. Also ensure security groups open neccessary ports
   or setup the SG for self-reference (allowing full access for all members of the SG)

3. Update the inventory and install_inventory.
   Update the templates/docker-storage-setup template variable {{ docker_storage_block_device }}
   if neccessary. Current value is /dev/xvdb for centos AMIs

4. Run prepare_hosts.yml with "inventory" as the inventory

5. Run the prerequisites.yml followed by deploy_cluster with the "install_inventory" playbook


