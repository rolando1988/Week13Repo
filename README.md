## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable,secure,available,efficient, in addition to restricting Traffic to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?

Load Balancers are crucial part of cybersecurity. They protect any cyber attacks from happening, attacks such as DoS attacks, and also helps programs to run smoothly. 

A jump box is a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments.
Jumoboxes apply an origanted point of access with high level security. 



Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- _TODO: What does Filebeat watch for?

Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to ElasticSearch or Logstash for indexing.


- _TODO: What does Metricbeat record?_

Metricbeat can record the collected metrics directly into Elasticsearch or send them to Logstash, Redis, or Kafka.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     |    Function    | IP Address | Operating System |
|----------|----------------|------------|------------------|
| Jump Box | Gateway        | 10.0.0.1   | Linux            |
| Web 1    | DVWA Webserver | 10.0.0.7   | Linux            |
| Web 2    | DVWA Webserver | 10.0.0.8   | Linux            |
| Web 3    | DVWA Webserver | 10.0.0.5   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Elk Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Public IP of your machine 

Public IP address
104.42.120.30

Machines within the network can only be accessed by the workstation and Jumbpbox.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

The webservers and ELK server can be accessed from the jumpbox VM via port 22 and SSH keys, though it may be necessary to create an inbound rule in their Security Group Settings.



A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Addresses |
|---------------|---------------------|----------------------|
| Jumpbox       | No                  | 10.0.0.4             |
| Web 1         | No                  | 10.0.0.7             |
| Web 2         | No                  | 10.0.0.8             |
| Web 3         | No                  | 10.0.0.5             |
| Load Balancer | Yes                 | 13.89.52.172         |
| ELK-Server    | No                  | 10.1.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

- Installs docker.io - It references the IP address listed under [elk] in ansible's hosts file to install docker on the target VM.
- Downloads and launches web container - Downloads and launches the ELK container, and lists the ports needed to access said container/application.
- Installs python3 -- the Docker module uses python
- Increases virtual memory - A standard container does not have enough virtual memory to run an ELK container. For 3 DVWA machines, the suggested amount is 262144.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring

Web-1 10.0.0.5
Web-2 10.0.0.6
Web-3 10.0.0.7

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed

ELK Server 
Web-1 
Web-2
The ELK Stack installed: Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.

Filebeat: Log Events
Metricbeat: Metrics and system statics
Using Filebeat allows you to monitor the log files or locations that you specify.
Using Metricbeat allows you to monitor your servers by collecting system metrics and services running on the server.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the Ansible ELK Installation and VM Config file to /etc/ansible/roles/files.
- Update the filebeat-configuration file to include e the ELK private IP in lines 1106 and 1806.
- Run the playbook, and navigate to ELK-VM Public IP to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? 

filebeat-playbook.yml

- Where do you copy it?

/etc/ansible/roles

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

Two separate groups in the /etc/ansible/hosts file.
One of the groups will be Webservers which have the IPs of the VMs where Filebeat was installed to. The other group is
named The-ELK-Server-VM which contains the IPs of the VM ELK will be installed to.

- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

-------Filebeat---------
- To create the filebeat-configuration.yml file: nano filebeat-configuration.yml. For this, I
used the filebeat configuration file template.
- To create the playbook: nano filebeat-playbook.yml
---
- name: installing and launching filebeat
hosts: webservers
become: true
tasks:
- name: download filebeat deb
command: curl -L -O
https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.1-amd64.deb
- name: install filebeat deb
command: dpkg -i filebeat-7.7.1-amd64.deb
- name: drop in filebeat.yml
copy:
src: ./files/filebeat-configuration.yml
dest: /etc/filebeat/filebeat.yml
- name: enable and configure system module
command: filebeat modules enable system
- name: setup filebeat
command: filebeat setup
- name: start filebeat service
command: service filebeat start
---
-To run the playbook: ansible-playbook filebeat-playbook.yml
* In order to run the playbook, you have to be in the directory the playbook is at, or give
the path to it (ansible-playbook /etc/ansible/roles/filebeat-playbook.yml
-------Metricbeat-------
- To create the metricbeat-configuration.yml file: nano metricbeat-configuration.yml. For
this, I used the metricbeat configuration file template.
- To create the playbook: nano metricbeat-playbook.yml
---
- name: installing and launching metricbeat
hosts: webservers
become: true
tasks:
- name: download metricbeat deb
command: curl -L -O
https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.7.1-amd64.deb
- name: install metricbeat deb
command: sudo dpkg -i metricbeat-7.7.1-amd64.deb
- name: drop in metricbeat.yml
copy:
src: /etc/ansible/roles/files/metricbeat-configuration.yml
dest: /etc/metricbeat/metricbeat.yml
- name: enable and configure system module
command: metricbeat modules enable system
- name: setup metricbeat
command: metricbeat setup
- name: start metricbeat service
command: service metricbeat start
---
- To run the playbook: ansible-playbook metricbeat-playbook.yml
* To order to run the playbook, you have to be in the directory the playbook is at, or give
the path to it (ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml