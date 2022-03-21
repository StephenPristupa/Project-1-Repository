# Project-1-Repository
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/89100194/159202858-626614b1-0547-4bf3-8dfb-4f1463e9f9c5.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  -[](/Ansible)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting unauthorized traffic to the network.
- Load balancers affect the A or availablity of the CIA triad, ensuring that the app is always accessible even if one component goes down. In this case for instance, if one DVWA VM goes down, the load balancer can shift the traffic to the other VM.
- A Jump Box Provisioner is what is used here to quickly deploy new pre-configured containers to the associated VMs, rather than having to individually configure each system.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system files.
- Filebeat is used to monitor logs in a system, then forwards and indexes them with Elasticsearch.
- Metricbeat also sends data to Elasticsearch, but instead monitors the metrics and statistics of services running on a given system.

The configuration details of each machine may be found below.

| Name          | Function       | IP Address    | Operating System |
|---------------|----------------|---------------|------------------|
| JumpBox       | Gateway        | 10.0.0.5      | Linux            |
| Web-1         | DVWA 1         | 10.0.0.8      | Linux            |
| Web-2         | DVWA 2         | 10.0.0.9      | Linux            |
| ELK           | Kibana         | 10.1.0.4      | Linux            |
| Load Balancer | Load Balancing | 20.228.243.177| Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the LoadBalancer machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My Public IP (99.237.69.42)

Machines within the network can only be accessed by the Jump Box Provisioner.
- I allowed the Jump Box Provisioner to access my ELK VM via SSH. The IP is 10.0.0.5. I also allowed access for my public IP to kibana through port 5601.

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Addresses     |
|---------------|---------------------|--------------------------|
| Jump Box      | No                  | My public IP             |
| Web-1         | No                  | 10.0.0.5, 20.228.243.177 |
| Web-2         | No                  | 10.0.0.5, 20.228.243.177 |
| ELK           | No                  | 10.0.0.5, My public IP   |
| Load Balancer | Yes                 | My public IP             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantage of automating configuration of the ELK machine is that this action is easily scalable and repeatable should the need for additional machines arise.

The playbook implements the following tasks:
- Installs the Docker module in order to configure and check containers
- Enables the installation of python packages not in the standard library
- Installs the python module
- Increases the amount of memory that can be used by the module
- Downloads and launches an ELK container
- Enables the docker service on machine bootup so that it doesn't have to be manually restarted each time

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://user-images.githubusercontent.com/89100194/159203066-011575d9-992c-40eb-9c3b-90be69e2453d.png)

### Target Machines & Beats

This ELK server is configured to monitor the following machines:
- 10.0.0.8
- 10.0.0.9

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat collects logs about system files which we use to track when and by whom files are modified. This especially helps ensure the integrity of files, as we can track if they have been modified by an unauthorized user.

- Metricbeat monitors system metrics and service metrics to determine if everything is running as it should be. It can be used to notify us if a system or service is using more resources than it should be, after which we can troubleshoot and determine if this is due to an attacker or not.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /roles.

- Update the playbook file to include the necessary modules and commands for it to call when you run the ansible-playbook '<playbook_file.yml>' command

- Run the playbook, and navigate to Kibana to check that the installation worked as expected.
 
- The playbook file is in .yml format, and you copy it into the /roles directory in order for it to be called by the config file.

- You update the /etc/ansible/hosts file with groups such as [webservers] or [elk] with the associated IPs, then you can call these groups in the yaml script in order to ensure it only operates on the intended machines.

- You can navigate to http://<ELK_VM_IP>:5601/app/kibana in order to see data from the ELK server.
