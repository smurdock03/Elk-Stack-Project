## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

(Images/https://github.com/smurdock03/Elk-Stack-Project/blob/main/diagrams/Elk%20Stack%20Diagram.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _filebeat.yml_ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

  
---
- name: Install Elk Server Components
  hosts: elk
  become: true
  tasks:

  - name: set Max map count to 262144 in sysctl
    sysctl:
      name: vm.max_map_count
      value: 262144

  - name: Install Docker
    apt:
      name: docker.io
      state: present
      update_cache: yes

  - name: Install PythonPip
    apt:
      name: python3-pip
      state: present

  - name: install pip docker
    pip:
      name: docker
      state: present

  - name: install docker elk container
    docker_container:
      name: elkV761
      image: sebp/elk:761
      state: started
      ports:
       - "5601:5601"
       - "9200:9200"
       - "5044:5044"
      restart_policy: always

  - name: start docker on startup
    systemd:
      name: docker
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

Load balancing ensures that the application will be highly _accessible_, in addition to restricting _access_ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box? Load Balancers Protect against DDoS attacks. The jumpbox adds an extra layer of protection behind a firewall to ensure multiple layers of protection

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _Jumpbox_ and system _network__.
- _TODO: What does Filebeat watch for?_ Log files and file name changes
- _TODO: What does Metricbeat record?_Collects Metrics from the OS and services that are running on the servers

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function   | IP Address | Operating System |
|----------|------------|------------|------------------|
| Jump Box | Gateway    | 10.3.0.4   | Linux            |
| Elk VM   | Monitoring | 10.1.0.4   | Linux            |
| Web-1    | Webserver  | 10.3.0.6   | Linux            |
| Web-2    | Webserver  | 10.3.0.7   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _Jumpbox Provisioner Machine_ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: my own public IP address
- _TODO: Add whitelisted IP addresses_5601 Kibana Port

Machines within the network can only be accessed by the Jumpbox Provisioner.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_67.187.71.243

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |  67.187.71.243       |
|   Web-1  | No                  |  10.3.0.4            |
|   Web-2  | No                  |  10.3.0.4            |
|  Elk VM  | No                  |  10.3.0.4            |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_Ansible allows you to send plays (YAML playbooks in which you can configure numerous commands) to a network of computers instead of doing one at a time. It's automation at it's finest

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ... Install docker.io
- ... Install pip3
- ... Install Docker Python module
- ... Increase virtual memory
- ... Download and the launch docker 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_Web-1 10.3.0.6 and Web-2 10.3.0.7

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_ Metricbeats

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._I noticed that the Metricbeat was tracking the PostgreSQL. Its an open source database sytem. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _playbook_ file to _Ansible container_.
- Update the _playbook_ file to include...
- Run the playbook, and navigate to _Web-1 and Web-2 VMs_ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? I have 4 different playbooks. each has a different purpose. It helped me locate erros while creating the playbooks. they are all located in the ansible folder within my github. Where do you copy it?_You have to ensure you are in your anisble container, mine was called angry_rosalind
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_You want to make sure that the Filebeat is installed on the webservers. You can set that up in the configuration folders and assign the filebeat to Web-1 and 2. You want to monitro the trafffic there and hold the logs and track everything on your elk stack- _Which URL do you navigate to in order to check that the ELK server is running? http://20.150.140.208:5601/app/kibana#/home

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._