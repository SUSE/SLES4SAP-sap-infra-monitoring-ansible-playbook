# SLES4SAP-sap-infra-monitoring-ansible-playbook

 This ansible playbook is deploying most of the monitoring solutions discussed in the Best Practice Paper:<br>
 [Infrastructure monitoring for SAP Systems](https://documentation.suse.com/sbp/sap-15/html/SBP-SLES4SAP-sap-infra-monitoring/index.html).
 

 ## Components:
 The following components are currently implemented:   

### Data Storage, Visualization and Notification (server component):
* Grafana Dashboard
* Prometheus Server
* Prometheus Alertmanager
* Loki Server

### Data Sources (agent component):
* Promtail 
* Prometheus Note Exporter
* Collectd
* PCM


## Customizable Settings:
The deployment should be adaptable to an existing infrastructure. Therefore different settings are configurable via default variables. 
These variables are stored under [/group_vars/all/main.yaml](group_vars/all/main.yaml).

The following settings are possible:

### Port Numbers:
Changing them will automatically change it to all depending components. 
If a firewall is active, ports will be automatically add (permit traffic).

### Arguments:
Some components needs different arguments which are typically implemented in sysconfig files.
These arguments can also easily changed within group_vars. 

## Hosts:
All needed hosts can be add to the [inventory.yaml](inventory.yaml) file. <br>
There should be only one host entry for each server component in the section "Server Hosts"
This can be either for each component a different host or one for all components.

Monitored hosts (agent hosts) will be automatically add to the server configurations <br>
(e.g. /etc/prometheus/prometheus.yaml)  

Variables starting with **deploy_** are used to choose if a component is enabled or not (true/false). They are set to **true** as a general default in the section **all**. <br>
Setting it to **false**  no host will be deployed with that component at all.

The variables **deploy_** can also be used to disable single hosts under a host entry. 

```
all:
  vars:
    deploy_node_exporter: true
    deploy_collectd: false

monitoring-agents:
  hosts:
    host01.example.com:
    host02.example.com:
      deploy_node_exporter: false
    host03.example.com:   
```
In the example above **collectd** will not be deployed at all. <br> 
The **node_exporter** will be installed on all hosts but not on **host02.example.com**. 


## Install and run Ansible:
Install ansible, playbook dependencies and finally run the playbook:

```
pip install ansible
ansible-galaxy collection install -r requirements.yaml
```

```
ansible-playbook -i inventory.yaml --user root playbook_monitoring.yaml
```
Please change [inventory.yaml](inventory.yaml) and  [/group_vars/all/main.yaml](group_vars/all/main.yaml) before run ansible.

