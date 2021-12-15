## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Red-team Network-diagram:

![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Diagrams/Redteam-topology-MH_Unit-13.drawio.png "Network Diagram")

![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/red-team-inbound.png "in-bound-rules")

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the (yaml) file may be used to install only certain pieces of it, such as Ansible, DVWA servers, or Elk Stack with Filebeat and Metricbeat.

  - _The playbook files:_ DVWA, Elk, Filebeat, metricbeat.

  [install-elk](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Ansible/install-elk.yml)

  [filebeat-playbook](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Ansible/filebeat-playbook.yml)

  [metricbeat](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Ansible/metricbeat-playbook.yml)

This document contains the following details:
- Description of the topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant/available, in addition to restricting access and or distributing traffic to the network.
- _What aspect of security do load balancers protect? What is the advantage of a jump box?_
  - Load Balance distributes traffic and protects against DDOS attacks.
  - Jump box and load balancers limit access to assets.
  - Distributes client requests or network load efficiently across multiple servers.
  - Ensures high availability and reliability by sending requests only to servers that are online.
  - Provides the flexibility to add or subtract servers as demand dictates.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system Metrics.
- _What does Filebeat watch for?_
  - Filebeat is a log data shipper for local files. Installed as an agent on your servers, Filebeat monitors the log directories or specific log files, tails the files, and forwards them either to Elasticsearch or Logstash for indexing. An example of such are the logs produced from the MySQL database supporting our application.

- _What does Metricbeat record?_
  - Metricbeat collects metrics and statistics on the system. An example of such is cpu usage, which can be used to monitor the system health.

_The configuration details of each machine may be found below._

| Name                | Function       | IP Address | Public IP     | Operating System |
|---------------------|----------------|------------|---------------|------------------|
| Jumpbox Provisioner | Gateway        | 10.0.0.4   | 23.99.178.156 | Linux            |
| web-1               | DVWA Container | 10.0.0.5   |               | Linux            |
| web-2               | DVWA Container | 10.0.0.6   |               | Linux            |
| wweb-3              | DVWA Container | 10.0.0.8   |               | Linux            |
| elk1                | config/monitor | 10.1.0.4   | 20.119.57.87  | Linux            |
|                     |                |            |               |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _Whitelisted IP addresses_
  - My `home public IP` to `Jump box provisioner` restricted inbound rule on port 22.
  - Machines within the network can only be accessed by ssh from the Ansible container on the jumpbox.

- _Machines allowed to access the ELK VM and its IP address._
  Jump server via peering connection using SSH. 10.0.0.4 to 10.1.0.4.
A summary of the access policies in place can be found in the table below.

|  Name              	|  Publicly Accessible 	|  IP Address    	|  Function               	|
|--------------------	|----------------------	|----------------	|--------------------------	|
|  Jump box          	|  Yes                 	|  23.99.178.156 	|  Container / provisioner 	|
|  Ansible Container  |  NO                   |                 |  SSH to Web servers       |
|  red-team-frontend 	|  Yes                 	|  40.83.47.118  	|  DVWA Load Balancer HTTP 	|
|  Web-1             	|  No                  	|  40.83.47.118  	|  DVWA webserver container	|
|  Web-2             	|  No                  	|  40.83.47.118  	|  DVWA webserver container	|
|  Wweb-3            	|  No                  	|  40.83.47.118  	|  DVWA webserver container	|
|  ELK Stack         	|  Yes                 	|  20.119.57.87  	|  Public HTTP             	|

Note: wweb-3 spelling intentional.

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _What are the main advantages of automating configuration with Ansible?_
  - Ansible allows quick and easy deployment of multitier applications via playbooks.
  - No custom code is necessary to automate the systems.
  - Ansible will also deploy and bring up the asset in the desired run state.

The playbook implements the following tasks:
- _The main steps of the ELK installation playbook, e.g., install Docker; download image; etc._
- ...- 	1. Check if docker.io is present, if not install it. (also install pip and python3)

   ![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/elk-play1.png "docker.io check")


- ...- 2. Set the Memory.

   ![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/elk-play2.png "set memory")



- ... - 3. Download and launch the elk container and define the published ports.

   ![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/elk-play3.png "download and launch")



- ... -	4. Enable service docker on boot.

   ![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/elk-play4.png "enable on boot")



_The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance._

  ![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/elk-docker-ps.png "elk-docker-ps")

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _The IP addresses of the machines being monitored_

| Server Name 	| IP Address  	|
|-------------	|------------	|
| Web-1       	| (10.0.0.5) 	|
| Web-2       	| (10.0.0.6) 	|
| Wweb-3      	| (10.0.0.8) 	|


_We have installed the following Beats on these machines:_
 - ... -Filebeat:
![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/filebeat-playbook-deploy.png "filebeat deploy")

- ... -Metricbeat

![alt test](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/metricbeat-playbook-deploy.png "merticbeat deploy")

_These Beats allow us to collect the following information from each machine:_
 - Filebeat is a log data shipper for local files. Installed as an agent on your servers, Filebeat monitors the log directories or specific log files, tails the files, and forwards them either to Elasticsearch or Logstash for indexing. An example of such are the logs produced from the MySQL database supporting our application.

 - Metricbeat collects metrics and statistics on the system. An example of such is cpu usage, memory and network statistics, which can be used to monitor the system health.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured.

_Configure the Jumpbox to run Docker with an Ansible control node._

- Start by installing `docker.io` on your Jump box.
 - Run `sudo apt update` then `sudo apt install docker.io`
 - Verify that the Docker service is running.
 - Run `sudo systemctl status docker`. (_Note: If the Docker service is not running, start it with `sudo systemctl start docker`._)
 - Once Docker is installed, pull the container **cyberxsecurity/ansible**.
 - Run sudo docker pull `cyberxsecurity/ansible`.
 - You can also switch to the root user so you don't have to keep typing sudo.
 - Run sudo su.
 - Launch the Ansible container and connect to it using the appropriate Docker commands.
 - Run `docker run -ti cyberxsecurity/ansible:latest bash` to start and connect to the container.

_Assuming you have such a control node provisioned:_

SSH into the control node and follow the steps below:
 - Copy the `yaml` file to `ansible folder`.
 - Update the `config file` to include `remote users and ports within the appropriate stanza`.
 - Run the playbook, and navigate to the public IP of the ELK server on the specified port to the kibana app. [http://20.119.57.87/app/kibana](http://20.119.57.87/app/kibana) to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it?_
  - Copy the `filename-playbook.yml` file to `/etc/ansible/files`. For instance, other yaml files would include web servers, elk, filebeat, and metricbeat.

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
  - Host-to-yaml corrolation: The host file defines the hosts, then the yaml file uses the host definition to specify where to direct the installation.
  
  - Define the host in  `/etc/ansible/hosts.cfg`.

![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/Ansible_hosts_stanzas.png?raw=true "hosts_config")

  - In the yaml file designate the desired `hosts` for the install.

![alttext](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/Host-define-yml.png?raw=true "host-define-yml")


  - Update the `filebeat-config.yml` file to include the ELK server private IP in lines `1106` and `1806`.
- _Which URL do you navigate to in order to check that the ELK server is running?
  - Elk-public-IP:5601/app/kibana


_**This section** provides the specific commands the user will need to run to download the playbook, update the files, etc._

-	Go to /etc/ansible/files and use the curl command to add the config file:
curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml

Copy the filebeat-playbook.yml file to /etc/ansible/.

Update the filebeat-config.yml file to include the ELK server private IP in lines 1106 and 1806.

Once you have this file on your Ansible container, edit it as specified:

The username is *elastic* and the password is *changeme*.
 - username:

![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/elastic_line-1106.png?raw=true "elastic_line-1806")

Scroll to line ``#1106`` and replace the IP address with the IP address of your ELK machine.
- kibana IP:

![alt text](https://github.com/mhighbe-20/cybersecurity-Elk_project1/blob/main/Images/elastic_line-1806.png?raw=true "line 1106")


Run the filebeat-playbook.yml playbook, and navigate to the kibana page at [ELK public IP]/app/kibana to check that the installation worked as expected.

-	Navigate to your ELK server's IP.
-	Filebeat
1.	Click Add Log Data.
2.	Choose System Logs.
3.	Click on the DEB tab under Getting Started.
4.	Step 5: Module Status and click Check Data.

-	metricbeat
1.	Click Add Metric Data.
2.	Click Docker Metrics.
3.	Click the DEB tab under Getting Started for the correct Linux instructions.
4.	Step 5: Module Status and click Check Data.
