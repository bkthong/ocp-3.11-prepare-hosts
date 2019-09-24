# ocp-3.11-prepare-hosts
For me to use in aws for preparing ec2 instances as ocp nodes

- Nodes will be using centos 7 amis
- inventory is static at the moment. When using, update with list of ec2 instances you will
  be using as ocp nodes
- Expects the private key /home/ec2-user/.ssh/aws-jase.pem to exist on the control node
  and this key can be used to access all the nodes
- Docker storage volume will be configured to use overlay2, using the {{ docker_storage_block_device }} 
  inventory variable to identify the available device for docker to use as storage
