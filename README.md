# andy
andy works
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/andsew/andy/blob/main/Diagrams/Project%20One%20%7BAM%7D%20Diagram.drawio.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible playbook file may be used to install only certain pieces of it, such as Filebeat.

  - filebeat-config.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

- Load balancer help the flow of healthy traffic to the web server machines which mitigates Dos attacks by shifting attacking traffic.
- The jump box is exposed to the public and is the gateway to the network allowing only known IP addresses. The jump box is a single point of entry which allows to manage user accounts. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system files.

- Filebeat collects data about the file system as log files data and organize them and sends it to Logstash and Elasticsearch. It helps views 
- Metricbeat collects data about the system of a machine and service metrics for instance the uptime, which helps monitor the system and processes running on it.

The configuration details of each machine may be found below.


| Name                 | Function   | IP Address  | Operating System     |
|----------------------|------------|-------------|----------------------|
| Jump-Box-Provisioner | Gateway    | 10.0.0.4    | Linux (Ubuntu 18.04) |
| Web-1                | Web Server | 10.0.0.5    | Linux (Ubuntu 18.04) |
| Web-2                | Web Server | 10.0.0.7    | Linux (Ubuntu 18.04) |
| ELKVM                | Server     | 10.2.0.4    | Linux (Ubuntu 18.04) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 73.179.123.282 which is the allowed user IP in the Network Security Group white list.

Machines within the network can only be accessed by Jump-Box-Provisioner.

- The-Jump-Box-Provisioner Machine is the only machine with allowed access with Vnet IP address of 20.121.26.242"  

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses |
|----------------------|---------------------|----------------------|
| Jump-Box-Provisioner | Yes                 | 73.179.123.282       |
| Web-1                | No                  | 10.0.0.4             |
| Web-2                | No                  | 10.0.0.4             |
| ELKVM                | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- Using Ansible saves valuable time configuring and setting up the Elk server, additionally this is done from one place using YAML Playbooks.

- Automating configuration with Ansible allows to configure, download and launch the server with the help of the Ansible playbooks and trouble shoot and check if all run as desired. 

The playbook (install-elk.yml) implements the following tasks:

- First SSH into the Jump-Box-Provisioner (sshreddmin@20.121.26.242)
- Start and Attach to the ansible docker (sudo docker start vigorous_moore), (sudo docker attach vigorous_moore)
- Access the directory /etc/ansible and create install-elk.yml
 - apt installs docker 
 - apt installs python3-pip 
 - pip installs Docker Module 
 - increase the VM Memory to 262144
 - download and launch docker elk container with set of port mapping"
 - enable the docker service on boot
- Run the install-elk.yml (ansible-playbook install-elk.yml)
  
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)



### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- VM Web-1 (IP:10.0.0.5) 
- VM Web-2 (IP:10.0.0.7) 

We have installed the following Beats on these machines:

- Filebeat (filebeat-config.yml, filebeat-playbook.yml)
- Metricbeat (metricbeat-config.yml, metricbeat-playbook.yml)

These Beats allow us to collect the following information from each machine:

- Filebeat collects log events in a log files or locations specified and forwards it to Elasticsearch or Logstash for indexing and is able to viewed and analyzed in Kibana platform.

- Metricbeat helps monitor Web-1 and Web-2 servers by collecting machine metrics from their systems and services running on the servers. The system collections can be from the cpu, memory, network, uptime and etc, as for the services running it can be Nginx, Apache, MySQL and etc. The machine metric collections helps analyze the health of the machine.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

Filebeat

- Copy the filebeat-config.yml file to /etc/ansible/files/filebeat-config.yml.
- Update the filebeat-config.yml file to include the private IP:10.2.0.4 of the ELK VM

output.elasticsearch:
hosts: ["10.2.0.4:9200"]
username: "elastic"
password: "changeme"

AND

setup.kibana:
host: "10.2.0.4:5601"


- Run the playbook, and navigate to http://[yourELK-VM_External_IP]:5601/app/kibana (http://104.40.18.3:5601/app/kibana) to check that the installation worked as expected.

Metricbeat 

- Copy the metricbeat-config.yml file to /etc/ansible/files/metricbeat-config.yml.
- Update the metricbeat-config.yml file to include the private IP of the ELK VM as we did for the filebeat.
- Run the playbook, and navigate to http://[yourELK-VM_External_IP]:5601/app/kibana

Answer the following questions to fill in the blanks:

- Which file is the playbook?

filebeat-playbook.yml

- Where do you copy it?

 /etc/ansible/roles

- Which file do you update to make Ansible run the playbook on a specific machine? 

Hosts file at /etc/ansible/hosts

How do I specify which machine to install the ELK server on versus which to install Filebeat on?

Edit the host file at /etc/ansible/hosts and include

[webservers]

10.0.0.5 ansible_python_interpreter=/usr/bin/python3
10.0.0.7 ansible_python_interpreter=/usr/bin/python3

[elk]

10.2.0.4 ansible_python_interpreter=/usr/bin/python3

- Which URL do you navigate to in order to check that the ELK server is running? 

http://[your.ELK-VM.External.IP]:5601/app/kibana, the public IP of ELK VM is dynamic it keeps on changing when started from the Azure portal as such its new every time.

- Commands needed to create the filebeat-playbook.yml run, to download, update the files, etc

On terminal navigate to /etc/ansible/roles and create a YAML file: nano filebeat-playbook.yml

- YAML File structure 
 - ---
 - name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:
  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.6.1-amd64.deb

  - name: drop in filebeat.yml 
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start

  - name: enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes


- To run filebeat-playbook.yml:

Ansible-playbook filebeat-playbook.yml

- The same structure and commands can be used to download and run Metricbeat-playbook.yml

