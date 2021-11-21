## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

 ~/Diagrams/Cloud-Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible file may be used to install only certain pieces of it, such as Filebeat.
elk.yml
` ---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: sysadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes
  - _filebeat-playbook.txt 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build `


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly stable, in addition to restricting access to the network.
- _Load balancers take traffic from clients across your servers without the clients needing to understand how to operate them.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jumpboxes and system Machines.
- _TODO: What does Filebeat watch for?_ File Changes
- _TODO: What does Metricbeat record?_ Records the metrics from the running system and other services.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name       | Function        | IP       | OS    |
|------------|-----------------|----------|-------|
| Jumpbox    | Gateway         | 10.0.0.5 | Linux |
| Web-VM-1   | Web Machine     | 10.0.0.6 | Linux |
| Web-VM-2   | 2nd Web machine | 10.0.0.7 | Linux |
| Project-VM | Monitor         | 10.1.0.4 | Linux |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 5601 Kibana

Machines within the network can only be accessed by jumpbox.
My machine - 76.30.46.47
A summary of the access policies in place can be found in the table below.

| Name       | Function        | IP       | OS    |
|------------|-----------------|----------|-------|
| Jumpbox    | Gateway         | 10.0.0.5 | Linux |
| Web-VM-1   | Web Machine     | 10.0.0.6 | Linux |
| Web-VM-2   | 2nd Web machine | 10.0.0.7 | Linux |
| Project-VM | Monitor         | 10.1.0.4 | Linux |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _Simple setup, can automate configs, Flexible, can be used without being manned.
The playbook implements the following tasks:
-	Docker.io
-	Pip3
-	Docker python module
-	Increase memory
-	Launch docker


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

 

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _10.0.0.6
10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
Metric Beat

These Beats allow us to collect the following information from each machine:
- _TODO: Filebeat- collects data about the system
Metricbeat-collects machine metrics

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: SSH Into your ansible 



SSH into the control node and follow the steps below:
- Copy the playbook file to ansible control node.
- Update the hosts file to include webservers and elk
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

