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


## General Customizable Settings:
The deployment should be adaptable to an existing infrastructure. Therefore different settings are configurable via default variables. 
These variables are stored under [/group_vars/all/main.yaml](group_vars/all/main.yaml).

The following settings are possible:

### Port Numbers:
Changing them will automatically change it to all depending components. 
If a firewall is active, ports will be automatically add (permit traffic).

### Arguments:
Some components needs different arguments which are typically implemented in sysconfig files.
These arguments can also easily changed within group_vars. 

## Inventory file:
All needed hosts can be add to the [inventory.yaml](inventory.yaml) file.

### Server Host Groups:
There should be only one host entry for each server group (e.g. grafana_server, prometheus_server, ...) in the section "Server Hosts"
This can be either for each component a different host or up to one for all group.

### Agent Host Groups:
Systems to be monitored (agent_xyz) will be automatically add to the server configurations (e.g. /etc/prometheus/prometheus.yaml)
They can be grouped into different host group. Important is that the group name has to start with:

```
agents_
```
**IMPORTANT:** Having the same host in different **Agent Host Groups** is not allowed.  

### Fine tuning

#### Deployment in general  
Variables starting with **deploy_** are used to choose if a server or component is enabled or not (true/false). They are set to **true** as a general default in the section **all**. <br>
Setting it to **false**  no host will be deployed with that component at all all.

The variables **deploy_** can also be used to disable/enable components for single hosts under a host entry. <br>Here is a example:

```
all:
  vars:
    deploy_node_exporter: true
    deploy_collectd: false

agents_kvm:
  hosts:
    host01.example.com:
    host02.example.com:
      deploy_node_exporter: false
    host03.example.com:   
```
In the example above **collectd** will not be deployed at all. <br> 
The **node_exporter** will be installed on all hosts but not on **host02.example.com**. 

If for a **Server Component** is either no host given or the deploy_ variable is set to false the service will be stopped and the firewall port dissabled (In case it was running before). This is also true for all depending exporter (for Prometheus) or promtail services (for Loki) on the agent hosts.  

#### Grafana Dashboard
* Default configuration can be changed in the section **Grafana Dashboard** under [/group_vars/all/main.yaml](group_vars/all/main.yaml)

#### Prometheus Server
* Default configuration can be changed in the section **Prometheus** under [/group_vars/all/main.yaml](group_vars/all/main.yaml).
* Any additional exporter configuration can be add to the end of the [prometheus.yaml](/roles/prometheus_server/templates/prometheus.yml) template file.
* Additional rule files should be add to the folder **/roles/prometheus_server/templates/rules/*.yaml**
* If the variable **deploy_prometheus_alertmanager** (inventory.yaml) is set to **false** or there is no host entry under the host group **prometheus_alertmanager** the **alerting section** in the prometheus.yml file will be removed. 

#### Prometheus Alertmanager
* Default configuration can be changed in the section **Prometheus Alertmanager** under [/group_vars/all/main.yaml](group_vars/all/main.yaml)
* The file [alertmanager.yml](/roles/prometheus_alertmanager/templates/alertmanager.yml) contains a very basic example mailserver configuration and needs to be adapted to your needs.  

#### Loki Server
* Default configuration can be changed in the section **Loki** under [/group_vars/all/main.yaml](group_vars/all/main.yaml)



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

