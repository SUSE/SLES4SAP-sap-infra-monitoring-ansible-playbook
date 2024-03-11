# Infrastructure-monitoring-for-SAP-Systems-ansible-playbook

 This ansible playbook is deploying most of the monitoring solutions discussed in the Best Practice Monitoring Paper.
 
 
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


## Customizable Settings:
The deployment should be adaptable to an existing infrastructure. Therefore different settings are configurable via default variables.<br> 
These variables are stored under [/roles/configuration/vars/default.yaml ](roles/configuration/vars/default.yaml).

The following main settings are possible:

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


