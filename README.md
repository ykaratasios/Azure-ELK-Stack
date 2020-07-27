# Homework_13
Git Repository of all files used in creating an ELK Stack with dockers and azure virtual machines.


To begin this project you first need to create: a Resource Group, a Virtual Network, a Network Security Group, and three virtual machines.

Starting off with the Resource Group:
  Search for resource group
  Click "Create Resource Group"
  Choose any name you want.
  Review and Create
  Create
  
Virtual Networks
  Search for Virtual Networks
  Choose any name you want
  Keep everything at its default
  Create
  
Network Security Group
  Create New Security group
  Add this group to your resource group
  Make sure this group is in the same region as before
  
Virtual Machines
  NOTE: create an SSH key pair before this begins
  
  Basic
    Search for Virtual Machines
    Resourse Group: use the one created prior
    Name this one "Jump Box Provisioner"
    Region: same as before
    Image: Ubuntu Server 18.04
    Size: Standard B1s
    Authentication Type: SSH pub key
    Username: something that you can remember
    SSH public key: copy and paste your SSH public key
  Networking
    Virtual Network: the one created prior
    Subnet: default
    Public IP: (new)
    NIC network security group: advanced
    Configure network security group
    
  Create and Review
    Create
    
Set a Firewall rule to allow for SSH connections
  Choose inbound security rules
  +Add
    source: IP Addresses with your local IP address
    source port ranges: *
    destination: Any or Virtual Network
    dest port ranges: 22
    protocol: Any or TCP
    Action: Allow
    Priority: anything below 4096
    
Log into Jumpbox
  sudo apt install docker.io
  sudo docker pull cyberxsecurity/ansible
  
Set a Firewall rule to allow for SSH connections
  Choose inbound security rules
  +Add
    source: IP Addresses with your Jumpbox IP address
    source port ranges: *
    destination: Virtual Network
    dest port ranges: 22
    protocol: Any or TCP
    Action: Allow
    Priority: anything below the previous rule
    
In the Jumbox VM
  Run docker run -it cyberxsecurity/ansible /bin/bash
    run ssh-keygen
    cat .ssh/id_rs.pub
    copy the string
    
  Create a new VM DVWA-VM1
    Avalability options: avalability set and create new
    user containers pubkey for the SSH user

Add the Ansible folder to your ansible container
  Change any IP addresses or account names necessary.
  
Create a New VM for the Elk Server
  Create a new Ubuntu VM with:
    RAM: 4 GB+
    IP Address: must have public IP
    Networking: same network security group and Virtual Network
    Access: same SSH keys as the Ansible container and existing jump box.
    
In the Ansible/hosts
  Change the "elkserver" ip address to the private ip address of your VM
  
Run the Install-elk.yml file
  Check that it is open by going to http://[your.VM.PUblic_IP]:5601 on your local machine
  
In Ansible folder:
  open the filebeat-configuartion and edit line 
  #1106
    hosts:["localip:9200"] 
  #1806
    host: "localip:5601"]

Run the filebeat-playbook.yml
