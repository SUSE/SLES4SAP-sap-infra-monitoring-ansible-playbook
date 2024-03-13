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
These variables are stored under [/roles/configuration/vars/default.yaml](roles/configuration/vars/default.yaml).

The following settings are possible:

### Port Numbers:
Changing them will automatically change it to all depending components. 
If a firewall is active, ports will be automatically add (permit traffic).

### Arguments:
Some components needs different arguments which are typically implemented in sysconfig files.
These arguments can also easily changed within default.yaml. 

## Hosts:
All needed hosts can be add to the [inventory.yaml](inventory.yaml) file. <br>
There should be only one host entry for each server component in the section "Server Hosts"
This can be either for each component a different host or one for all components. 
All monitored hosts (agent hosts) will be automatically add to the server configurations (e.g. /etc/prometheus/prometheus.yaml)  


## Install and run Ansible:
Install ansible, playbook dependencies and finally run the playbook:

```
pip install ansible
ansible-galaxy collection install -r requirements.yaml
ansible-playbook -i inventory.yaml --user root playbook.yaml
```

Please change [inventory.yaml](inventory.yaml), [playbook-monitoring.yaml](playbook-monitoring.yaml) file and  [/roles/configuration/vars/default.yaml](roles/configuration/vars/default.yaml) before run ansible.

