# CybersecurityBC-Pj1
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Project 1 Red-Team Network Diagram](https://drive.google.com/file/d/1aCzZmHS7-iFd_-BD4aM_zT_U4Uxtk9i0/view?usp=sharing)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Project 1 Red-Team Network Diagram file may be used to install only certain pieces of it, such as Filebeat.

  - [Filebeat-playbook.yml](https://github.com/Geoff-Moore007/CybersecurityBC-Pj1/blob/main/Filebeat_playbook)
  - [Filebeat-config.yml](https://github.com/Geoff-Moore007/CybersecurityBC-Pj1/blob/main/Filebeat_config)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly functional, in addition to restricting high-traffic to the network.
- What aspect of security do load balancers protect? 
  - It helps prevent overloading servers as well as optimizes productivity and maximizes uptime. 
  - It also adds resiliency by rerouting live traffic from one server to another causing it to eliminate single points of failure from attacks such as DDoS attack.
	
- What is the advantage of a jump box?
  - Jump-box are highly secured computers that are never used for non-admin tasks. 
  - Throughout the years, jump-box has improved into an even more comprehensive/lock-down secure admin workstation to decrease the chances of hackers/malware infection. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the netwrok and system logs.
- What does Filebeat watch for?
  - It monitors the log files/locations that you specify and forwards them to Elasticsearch/Logstash for indexing.

- What does Metricbeat record? 
  - It records metrics/statistics data and transports them to the output that you specifics thru Elasticsearch/Logstash.

The configuration details of each machine may be found below.

| Name                | Function | IP Address                                 | Operating System |
|---------------------|----------|--------------------------------------------|------------------|
| JumpBox Provisioner | Gateway  | 10.0.0.4 (Private)//52.170.199.73 (Public) | Linux            |
| Elk-Server          | Server   | 10.1.0.4 (Private)//23.99.81.230 (Public)  | Linux            |
| Web-1               | Server   | 10.0.0.5 (Private)                         | Linux            |
| Web-1               | Server   | 10.0.0.6 (Private)                         | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address:
    
    - 70.175.145.34 (LocalHost IP Address)

Machines within the network can only be accessed by the JumpBox Provisioner.

- What machine(s) was/were allowed to access the Elk Server?
  - JumpBox Provisioner
 
- What was the IP address?
  - 10.0.0.4 (Private) // 52.170.199.73 (Public)1

A summary of the access policies in place can be found in the table below.

| Name                | Publicly Accessible | Allowed IP Address |
|---------------------|---------------------|--------------------|
| JumpBox Provisioner | Yes                 | 70.175.145.34      |
| Elk-Server          | No                  | 10.0.0.4           |
| Web-1               | No                  | 10.0.0.4           |
| Web-1               | No                  | 10.0.0.4           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because when automating scripts, not only does it take out the possible human error when setting up new servers, but it can also be quickly destroyed and/or setup in case one server goes down.

The playbook implements the following tasks:
	- First I, SSH into the Jump-Box-Provisioner (ssh sysadmin@52.170.199.73)
	- Went to /etc/ansible/roles directory and created the ELK playbook (Elk_Playbook.yml)
	- Ran the Elk_Playbook.yml in that same directory (ansible-playbook Elk_Playbook.yml)
  - Start/Attach to the ansible docker (sudo docker start wizardly_gauss)/(sudo docker attach wizardly_gauss)
	- Lastly, I SSH into the ELK-VM to verify the server is up and running.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

['docker ps' screenshot](https://github.com/Geoff-Moore007/CybersecurityBC-Pj1/blob/main/Elk%20Server%20Docker%20PS.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
  - Web-1 (10.0.0.5)
  - Web-2 (10.0.0.6)

We have installed the following Beats on these machines:
  - Filebeats
  - Metricbeats

These Beats allow us to collect the following information from each machine:
  - Filebeat is used to collect log files from specific files on remote machines.
	- Examples of Filebeats can be files that are generated by Apache, Microsoft Azure tools, the Nginx web server, and MySQl databases.

  - Metricbeat collects machine metrics. It is simply a measurement to tell analysts how healthy it is.
	- Examples of Metricbeat can be CPU usage/Uptime 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [Filebeat Configuration](https://github.com/Geoff-Moore007/CybersecurityBC-Pj1/blob/main/Filebeat_config) file to /etc/ansible/roles/files.
- Update the Filebeat_config file to include the Elk server private IP in lines 1106 and 1806.
- Run the playbook, and navigate to 23.99.81.230:5601 to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- Which file is the playbook?  filebeat-playbook.yml 
- Where do you copy it? /etc/ansible/roles
- Which file do you update to make Ansible run the playbook on a specific machine? /etc/ansible/hosts file (IP of the Virtual Machines).
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?  You will have to specify two separate groups in the etc/ansible/hosts file. One of the groups will be webservers which has the IPs of the VMs that you will install Filebeat on. The other group is named Elk Servers which will have the IP of the VM you will install ELK on.
- Which URL do you navigate to in order to check that the ELK server is running? 23.99.81.230:5601/app/kibana

As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

      -------Filebeat---------

	- To create the filebeat-configuration.yml file: nano filebeat-configuration.yml. For this, I used the filebeat configuration file template.
	
	- To create the playbook: nano filebeat-playbook.yml
	
      ---
 	 - name: installing and launching filebeat
    	   hosts: webservers
           become: true
           tasks:

    	   - name: download filebeat deb
      	     command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.1-amd64.deb

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
	
	* In order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/roles/filebeat-playbook.yml
	
	
	-------Metricbeat-------
	
	- To create the metricbeat-configuration.yml file: nano metricbeat-configuration.yml. For this, I used the metricbeat configuration file template.
	
	- To create the playbool: nano metricbeat-playbook.yml
	
	---
	  - name: installing and lunching metricbeat
	    hosts: webservers
	    become: true
	    tasks:
	    
	  - name: download metricbeat deb
	    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.7.1-amd64.deb
	    
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
	   
	   * To order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml
