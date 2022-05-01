## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/MeleMele9757/cyber-security-/blob/main/diagrams/project-net-map

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  ---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
    command: service filebeat start

    # Use systemd module
  - name: Enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting vulnerabilities to the network.
The load balancers help protect agianst DDOS attacks.
The use of a jumpbox creats a layer of security 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system permissions.
- filebeat monitors log data and forwards them to elasticsearch
-metricbeat takes the metrics and statistics and forwards them to elasticsearch

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| elk-host | server   | 10.1.0.4   | Linux            |
| web-1    | server   | 10.0.0.5   | linux            |
| web-2    | server   | 10.0.0.6   | linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox-provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
-96.41.147.34

Machines within the network can only be accessed by jumpbox-provisioner.
-20.187.110.123

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | 96.41.147.34         |
| elk-host | no                  | 20.187.110.123       |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because 
configurations can be made to multiple machines quickly without having to access them

The playbook implements the following tasks:
-install docker.io  
- install python3-pip
- install docker module

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/MeleMele9757/cyber-security-/blob/main/screen-shots/elk%20docker.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
load balancer ip 20.239.133.82
web-1 10.0.0.5
web-2 10.0.0.6

We have installed the following Beats on these machines:
filebeat
metricbeat

These Beats allow us to collect the following information from each machine:
log data
metric data and statistics


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible.
- Update the hosts file to include elk
- Run the playbook, and navigate to http://20.211.26.240:5601/app/kibana after starting the conatiner on the vm, to check that the installation worked as expected.


ssh <username>@<ip-of-jumpbox>
sudo docker container list -a
sudo docker start <specified-container>
sudo docker attach <specified-container>
#"copy playbook in apropriate directory"
ansible-playbook install-elk.yml
ssh <username>@<ip-for-host-vm>
sudo docker list container -a
sudo docker start <container-name>
then navigate to url