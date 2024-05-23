# Ansible-Playbooks

# Ansible-setup

https://github.com/vijay2181/ansible-playbooks

===========================================================
- take 3 t2.micro instances -> amazon linux
- 1 master 2 agents/nodes


1) Create amazon linux 3 Servers (1-AnsibleServer , 2- Host Machines for demo) in AWS.
 
2) Login As a root user and create ansible user and provide sudo access in all 3 Servers.
    2.1) Create the user ansible and set the password on all hosts:
           sudo useradd ansible
           sudo passwd ansible
     
     2.2) Make the necessary entry in sudoers file /etc/sudoers for ansible user for password-less sudo access in all 3 servers:      
           visudo
           ansible ALL=(ALL) NOPASSWD: ALL  

     2.3) Make the necessary changes  in sshd_config file /etc/ssh/sshd_config to enable password based authentication:
         Un comment PasswordAuthentication yes
         and comment  PasswordAuthentication no.
         And save the file .
            vi /etc/ssh/sshd_config

      2.4) Then restart sshd service.
           sudo systemctl restart sshd
           sudo systemctl status sshd


for passwordless authenctication
Generate SSH Key, Copy SSH Key(Ansible Server)
==============================================

1) Now generate SSH key in Ansible Server: 
  sudo su - ansible
  ssh-keygen

2) Copy public key to all agents/Host  servers as ansible user: 
   execute below below command on ansible master by updating HOST IP for all the HOST Servers. 
          ssh-copy-id ansible@172.31.34.56    ->  host 1 ip
          ssh-copy-id ansible@172.31.38.53    ->  host 2 ip
- if all 3 servers are within the same network, then private ip address will work
- test the connection
- ssh ansible@ip


Install Ansible in Master (Ansible Server)
==============================================
- switch to ansible user
sudo yum install python3 -y
sudo yum install python3-pip
pip3 install ansible
ansible --version


Update Host Inventory in Ansible Server to add agent/host servers’ details.
----------------------------------------------------------------------
1) Add Host Server details
  if you dont get /etc/ansible directory by default, then create it -> mkdir /etc/ansible

  sudo vi  /etc/ansible/hosts
  172.31.56.78   #agent1
  172.34.57.89   #agent2

 2) Use ping module to test Ansible and after successful run you can see the below output.

          ansible all -m ping

            172.31.35.23 | SUCCESS => {
            "changed": false,
             "ping": "pong"
             }
