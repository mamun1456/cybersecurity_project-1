## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)
 - Image is in Image folder as Diagram-Project1

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:
 
  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb
    
  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb
    
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

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly __secure___, in addition to restricting __DoS Attack___ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
Load Balancers - Load Balancing plays an important security role as computing moves evermore to the cloud. The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It does this by shifting attack traffic from the corporate server to a public cloud provider.

Jump Box - We can easily turn the ON/OFF remote desktop connectivity feature. By using the network security group, we can restrict the IP addresses to communicate with the Jump box. We can block the public IP address associated with the VM. It helps to improve security.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the __server___ and system __logs___.

ELK Stack is designed to allow users to take to data from any source. It also helps to find issues that occur in multiple servers by connecting their logs during a specific time frame.

- _TODO: What does Filebeat watch for?_
Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on servers. Filebeat monitors the log files or locations that we specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

- _TODO: What does Metricbeat record?_
Metricbeat is a lightweight shipper that we can install on our servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that we specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| ELKVM    | log event| 10.1.0.4   | Linux            |
| WEB-1    | Load Bal | 10.0.0.5   | Linux            |
| WEB-2    | Load Bal | 10.0.0.6   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the __ELK VM___ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_
104.215.76.43:5601

Machines within the network can only be accessed by __Jump Box___.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_
Jump Box, 10.0.0.4
ELK, 10.1.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | NO.                 | 10.0.0.5 10.0.0.6    |
|          |                     | 10.1.0.4                     |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

Ansible is fast and performs all functions over SSH and doesn't require agent installation. The main advantage is Ansible can be run from the command line without the use of configuration files for simple tasks, such as making sure a service is running, or to trigger updates or reboots

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- I started the dicker container and sensible.
- Then created an yml playbook inside ansible to install elk and run it
- I configured the ansible.cnfg and hosts files
- Then I create filbert yml file and then run it
- After successful running and installing, check the cabana site is up and data is flowing.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png) 
 - Image is in Image folder as Docker-ps

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
	10.0.0.5
	10.0.0.6

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
	ELK - 10.1.0.4

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

Filebeat: collects and ships log files.
Packetbeat: collects and analyzes network data.
Winlogbeat: collects Windows event logs.
Auditbeat: collects Linux audit framework data and monitors file integrity.
Heartbeat: monitors services for their availability with active probing.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ___filebeat.config__ file to __files directory__.
- Update the __filebeat___ file to include... IP address and port number.
- Run the playbook, and navigate to __Kibana site__ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ 
filebeat-configuaration.yml and copied it to files directory.

- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:
 
  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb
    
  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb
    
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

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
	- ansible.cfg and hosts.
	- ELK VM and JumpBox ansible container.


- _Which URL do you navigate to in order to check that the ELK server is running?
	- http://104.214.61.13:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._