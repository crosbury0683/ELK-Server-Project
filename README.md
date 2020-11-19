# Christopher-Rosbury-ELK-Server-Project

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML playbook files may be used to install only certain pieces of it, such as Filebeat.

My playbook files to install Elk, Filebeat and Metricbeat are accessible below:

* install-elk.yml
* metricbeat-playbook.yml
* filebeat-playbook.yml


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive and accessible, in addition to restricting users to the network.
What aspect of security do load balancers protect? What is the advantage of a jump box?

The load balancer can provide a web application firewall to protect the application from malicious actors, detect and block DDoS attacks and authenticate  
user access by requesting usernames and passwords from client devices. With regard to the CIA triad, a load balancer ensures availablity of an application.

The jumpbox controls access to machines on the private network. It allows access from specific IPs granted access in the network firewall rules that are set in the Network Security Group.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system logs and system metric data.

What does Filebeat watch for?

Filebeat watches for changes to system and application log files.

What does Metricbeat record?

Metricbeats records system metric data from the operating system installed on the server and running services on the server. Examples of the data that     
metricbeats records are CPU and RAM usage data, inbound and outbound network traffic and the number of processes running on the server. 

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System       |
|----------|----------|------------|------------------------|
| red-team1| Gateway  | 10.0.0.4   | Linux(ubuntu 18.04 LTS)|
| Web-1    | DVWA     | 10.0.0.5   | Linux(ubuntu 18.04 LTS)|
| Web-2    | DVWA     | 10.0.0.6   | Linux(ubuntu 18.04 LTS)|
| Web-3    | DVWA     | 10.0.0.7   | Linux(ubuntu 18.04 LTS)|
| ELK-VM   | ELK      | 10.1.0.4   | Linux(ubuntu 18.04 LTS)|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the red-team machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
My Public IP address

Machines within the network can only be accessed via SSH from the Ansible container labeled adoring_haibt.
Which machine did you allow to access your ELK VM? I provided access to the ELK through my Docker Container adoring_haibt What was its IP address? The container had no IP.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses         |
|----------|---------------------|------------------------------|
| red-team1| Yes/No              | My Public IP address         |
| Web-1    | No                  | Docker container on red-team1|
| Web-2    | No                  | Docker container on red-team1|
| Web-3    | No                  | Docker container on red-team1|
| ELK-VM   | Yes/No              | Public IP & red-team1 Docker |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually.
 What is the main advantage of automating configuration with Ansible?

Automating configuration is the main advantage of Ansible, provide ease of use.
Using YAML scripting allows for this thanks in part to being easily readable by non-technically inclined individuals.
Ansible can help with configuration and deployment of services on multiple servers & systems in far less tinme than performing these tasks manually.

The playbook implements the following tasks:
In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

- Create a Virtual Machine that will host the ELK server. The VM should have a public IP to access Kibana from our personal IP
- Add the ELK server's private IP to Ansible's hosts file under a 'elkservers' category to identify the private IP with the ELK server 
- Create an Ansible playbook file (.yml) that will install docker, python, download and launch ELK container on the ELK VM.
- You can create additioanal playbooks files to install Filebeat and Metricbeat
- Access the ELK server from the VM's public IP on port 5601, and allow monitored data to send to the Kibana web application through port 9200.

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:

- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

Filebeat monitors system log files and locations specified by the system admin, documents the log events and sends them to the ELK server for indexing. Filebeat logs
SSH logins from servers systems or virtual machines that it was configured to monitor.

Metricbeats records system metric data from the OS installed on the server and running services on the server.
Examples of the data are CPU and RAM usage, inbound and outbound network traffic and the number of processes running on the server.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the config file for both Filebeat and Metricbeat after downloading them using the 'curl' command to ~/etc/ansible/roles directory.


- Update the output.elasticsearch and setup.kibana parameters in both config files to use the private IP for the ELK server virtual machine.
- Run the playbook, and navigate to ELK VM via SSH to check that the installation worked as expected. Run 'sudo docker ps' command to check the installation.

  Which file is the playbook?
  
  - filebeat-playbook.yml
  - metricbeat-playbook.yml

   Where do you copy it?

   root@beac9613e6a2:/etc/ansible#, which is from the 'adoring_haibt' Docker container on the red-team 'Jump Box'

-Which file do you update to make Ansible run the playbook on a specific machine?

How do I specify which machine to install the ELK server on versus which to install Filebeat on?

Inside the /etc/ansible directory is a configuration file named hosts. That file has parameters for webservers, elkservers and other server types. In the install-elk playbook file, set the hosts parameter to elkservers so that the playbook knows to install the elk server container on the server that uses the elkservers parameter.

-Which URL do you navigate to in order to check that the ELK server is running?

 http://40.70.228.182:5601/ 

As a Bonus, I've provided the specific commands the user will need to run to download the playbook, update the files, etc.

* ssh username@publicJumpBoxIP
* sudo docker container ls -a
* sudo docker start (container ID number or name)
* sudo docker attach (container ID number or name)
* cd /etc/ansible
* open the hosts configuration file in a text editor and input the private IP for the ELK server under the elkservers parameter 
* download the Filebeat and/or Metricbeat config file using curl and store it locally (Ex. curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml) 
* change parameters in the output.elasticsearch and setup.kibana to use the private IP for your ELK VM
* create a playbook file (filebeat-playbook.yml) and specify the server type that the container will install on in the hosts paramter of the playbook file
* run ansible-playbook filebeat-playbook.yml 
* access http://ELKVMPublicIP:5601 in a web browser, click on 'Logs' from the Kibana menu to see Filebeat in action

The same process applies to Metricbeat. You will need to change file names to use Metricbeat, the URL to download the config file and click on 'Metrics' from the Kibana menu to see that beat in action.
