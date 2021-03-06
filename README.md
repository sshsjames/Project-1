# Azure Virtual Network: Project 1

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.  

![alt text](https://github.com/sshsjames/Project-1/blob/main/Images/WK-12-network.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.


This project contains the following details:
- Description of the Topology
- Access Policies / NSGs for the Azure network
- ELK Configurations
  - Beats in Use (Filebeat and Metricbeat)
  - Machines Being Monitored
- Answers to Questions regarding Kibana sample data
- (2) Interview questions selected from the choices given


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly dynamic, in addition to restricting access to the network.

-Question Posed: What aspect of security do load balancers protect? 
Load balancers work on layers 4-7 of the OSI model (transport, session application, presentation).  They often help to prevent DDoS attacks.  A load balancer will help to shift the attack from the corporate server, to the load balancer (that is more often in the cloud).  Additionally, the load balancers can be used to decrypt SSL traffic before it moves to the server, thus reducing computing power of the actual servers.  


-Question Posed: What is the advantage of a jump box?
A jump box or jump server makes things simple since it’s one place where direction and commands can be sent out to multiple devices.  This includes the ability for administrators to quickly make changes across multiple virtual machines and in some cases, across multiple regions at the same time.  The jump box is monitored and kept within a separate security zone than other devices.  This limits the exposure of the other virtual machines to the internet.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the configuration and system files.

-Question Posed: What does Filebeat watch for?
Filebeat is used to monitor log files from locations that the user specifies.  It collects these events and then forwards them on to Elasticsearch or Logstash for indexing, all the while being very lightweight.  


-Question Posed: What does Metricbeat record?
Metricbeat is similar to Filebeat in that it collects data and sends it over to programs such as Elasticsearch and Logstash.

The configuration details of each machine may be found below.

| Name    | Function   | IP Address | OS    |
|---------|------------|------------|-------|
| Jumpbox | Gateway    | 10.0.0.1   | Linux |
| Web-1   | Web server | 10.0.0.13  | Linux |
| Web-2   | Web server | 10.0.0.7   | Linux |
| Web-3   | Web server | 10.0.0.14  | Linux |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address:
 **MYIP**

Machines within the network can only be accessed by the Jump Box Provisioner.  The current configuration allows only for my personal IP to gain access to the ELK Virtual Machine by remoting into my Jump Box Provisioner and ultimately through the Ansible container.

A summary of the access policies in place can be found in the table below.


| Name     | Public Access | Allowed IP Addresses | OS    |
|----------|---------------|----------------------|-------|
| Jump Box | yes           | **MYIP**             | Linux |
| Web-1    | no            |  52.250.106.163      | Linux |
| Web-2    | no            |  52.250.106.163      | Linux |
| Web-3    | no            |  52.250.106.163      | Linux |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this allows for quick changes and implementation across multiple VMs with a simple change to the playbook within the ELK machine.  

# The playbook implements the following tasks:

The elk-playbook.yml file was created and explained in the following steps:

The first step or command is to install docker.io.  
Install python3-pip
Install Docker module
Increase virtual memory with vm.max_map
Use more memory
Download and launch Docker container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/sshsjames/Project-1/blob/main/Images/DockerPS.png) 

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.1
- 10.0.0.7
- 10.0.0.13
- 10.0.0.14

I have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat collects log data that is entering the network, while Metricbeat collects the metrics from the installed packages.


-Question Posed: Which URL do you navigate to in order to check that the ELK server is running?

- To check if its working, I used the following URL: http://104.43.228.135:5601/app/kibana


*NOTE I removed any actual personal IP addresses used to avoid that being published.  You will see it written as **MYIP** within the project pages.  
Additionally, it will be removed from all PNG files.  
