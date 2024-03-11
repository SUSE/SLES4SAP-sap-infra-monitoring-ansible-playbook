# Infrastructure-monitoring-for-SAP-Systems-ansible-playbook

## Playbook

```yaml
---
- name: monitoring deployment
  hosts:
    - monitoring-server
    - monitoring-agents 
  vars:
    prometheus-node_exporter-port = 9100
  roles:
    - monitoring-server
    - monitoring-agents  
```

## Invetory

```yaml
---
 monitoring-server:
   hosts:
     server-host01

 monitoring-agents:
   hosts:
     agent01
     agent02
     agent03            
```





## Monitoring-server
* Grafana
    * Install
    * Firewall Port 3000
    * Restart service
* Loki
* Prometheus
* Alertmanager



## Monitoring-agents
* Promtail
* Node exporter
* collectd
* Processor Counter Monitor (PCM)
* Prometheus IPMI Exporter


