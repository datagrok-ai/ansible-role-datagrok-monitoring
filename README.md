Role Name
=========
Monitoring

Install on one server next monitoring tools:  
* [Prometheus](https://prometheus.io/docs/introduction/overview/)
* [Grafana](https://grafana.com/grafana/)
* [Prometheus Alert Manager](https://prometheus.io/docs/alerting/latest/alertmanager/)
* [BlackBox Exporter](https://github.com/prometheus/blackbox_exporter)

Requirements
------------
OS:
 - Ubutnu/Debian based

Applicatons:
 - ansible [core 2.12.x]
 - python version = 3.8.x
 - docker version 20.10.x
 - docker-compose version 1.29.x
   
Deb packages:
 - software-properties-common
 - python3-pip
 - virtualenv
 - python3-setuptools 

Pip packages:
 - docker
 - docker-compose


Dependencies
--------------
 - Roles:
   - [geerlingguy.pip](https://github.com/geerlingguy/ansible-role-pip)
   - [cloudalchemy.node_exporter](https://github.com/cloudalchemy/ansible-node-exporter) 

Role Variables
--------------
| Variable | Default value | Description |
| -------- | ------------- | ----------- |
| **monitoring_server_inventory_group** | monitoring_manager | Group of hosts in your invenory, where monitoring will be hosted |
| **monitoring_targets_inventory_group** | monitoring_node | Group of hosts in your invenory, which will be monitored |
|**config_dir**| /etc/monitoring | Directory for storing <br> configuration files and running docker-compose.yml |
| **cvm_host** |  | [CVM](https://datagrok.ai/help/develop/admin/infrastructure#compute-components) server DNS <br> name or IP address, like <br> `https://somename.com`<br> or `http://1.2.3.4.` |
| **datagrok_host** |  | [Datagrok](https://datagrok.ai/help/develop/admin/infrastructure#datagrok-components) server DNS <br>name or IP address like <br>`https://somename.com`<br> or `http://1.2.3.4.` |
|**blackbox_endpoints**| `blackbox_endpoints:` <br>  `- "{{ cvm_host }}/grok_compute/info"`<br> `- "{{ cvm_host }}/jupyter/helper/info"`<br> `- "http://{{ cvm_host }}/notebook/helper/info"` <br> `- "http://{{ cvm_host }}/jupyter/api/swagger.yaml"` <br> `- "http://{{ cvm_host }}:5005/helper/info"` <br> `- "http://{{ cvm_host }}:54321/3/About"` <br> `- "http://{{ cvm_host }}/api/admin/health"` | Endpoints for monitoring <br> health checks on [CVM](https://datagrok.ai/help/develop/admin/infrastructure#compute-components)<br> and [Datagrok](https://datagrok.ai/help/develop/admin/infrastructure#datagrok-components) environment.<br> Use defaults or set up <br>needed value | 
|**monitoring_grafana_enabled**| true | Enable option to install<br> [ Grafana ]( https://grafana.com/grafana/ ). Describes <br>monitoring statistics in<br> graph mode. Can get <br>`true` or `false` value.<br> We recommend to import> `1860` dashboard for nodexporter, `14282` dashboard for cadvisor and `7587` dashboard for blackbox exporter|
|**monitoring_alertmanager_enabled**| true | Enable option to install <br>[Alert Manager](https://prometheus.io/docs/alerting/latest/alertmanager/). Used to <br>send monitoring alerts via e-mail and slack. Can get `true` or `false` value |
|**monitoring_blackbox_enabled**| true | Enable option to install<br> [ BlackBox Exporter ]( https://github.com/prometheus/blackbox_exporter ). Used for monitoring platform healthchecks |
|**monitoring_cadvisor_enabled**|true| Enable option to install <br>[ Cadvisor exporter ]( https://github.com/google/cadvisor ). Used for monitoring docker containers |
|**prometheus_image**| prom/prometheus:v2.40.4 | Used [ Prometheus ]( https://prometheus.io/docs/introduction/overview/ ) docker image |
|**grafana_image**| grafana/grafana-oss:9.2.6 | Used [ Grafana ]( https://grafana.com/grafana/ ) docker image |
|**alertmanager_image**| quay.io/prometheus/alertmanager:v0.24.0 | Used [ Alert Manager ]( https://prometheus.io/docs/alerting/latest/alertmanager/ ) <br>docker image |
|**blackbox_image**| prom/blackbox-exporter:master | Used [ BlackBox Exporter ]( https://github.com/prometheus/blackbox_exporter ) <br>docker image |
|**cadvisor_image**| gcr.io/cadvisor/cadvisor:v0.38.6 | Used [ Cadvisor exporter ]( https://github.com/google/cadvisor ) <br>docker image |
|**monitoring_node_exporter_port**| 9100 | External port for<br> [ Prometheus Node<br> exporter](https://prometheus.io/docs/guides/node-exporter/#monitoring-linux-host-metrics-with-the-node-exporter)  |
|**monitoring_cadvisor_port**|8181|External port for  [ Cadvisor exporter ]( https://github.com/google/cadvisor ) |
|**monitoring_prometheus_port**|9090|External port for [ Prometheus ]( https://prometheus.io/docs/introduction/overview/ ) |
|**monitoring_grafana_port**|3000|External port for [ Grafana ]( https://grafana.com/grafana/ ). First login/password is `admin/admin`|
|**monitoring_alertmanager_port**|9093|External port for [ Alert Manager ]( https://prometheus.io/docs/alerting/latest/alertmanager/ )|
|**monitoring_blackbox_port**|9115| External port for [ BlackBox Exporter ]( https://github.com/prometheus/blackbox_exporter ) |
|**slack_alerts**|true| Enable sending monitoring alerts via slack |
|**email_alerts**|true| Enable sending monitoring alerts via <br>e-mail |
|**group_message_interval**|5m| Time interval to send alerts |
|**repeat_message_interval**|30m| time interval for repeat alert message if alert still exists |
|**monitoring_slack_channel**|monitoring| Name of slack channel for sending alerts |
|**slack_webhook_url**|  | URL for using your Slack|
|**smtp_port**|25|Mail sending port|
|**smtp_login**|  |Login to SMTP mail server. Example: `name@mail.some`|
|**smtp_pwd**|  |Password to SMTP mail server.|
|**note_receiver**| `note_receiver:`<br> `-a@sd.com` <br> `-b@asd.com` |E-mail for alert receiving. Can have several values. |
|**sending_host**| |SMTP mail server IP address or DNS address|
|**send_resolved**|false|Receive resolved issues messages|
=======
Role Variables
--------------
| Variable | Description  |
| ------ | --------- |
| **monitoring_host_ip**    | Monitoring server DNS name or IP address. [Prometheus ]( https://prometheus.io/docs/introduction/overview/ ), [ Grafana ]( https://grafana.com/grafana/ ), [ Alert Manager ]( https://prometheus.io/docs/alerting/latest/alertmanager/ ), [ BlackBox Exporter ]( https://github.com/prometheus/blackbox_exporter ) will be deployed here   Use defaults or set up needed value in */vars/main.yml:* <br> <br>  `monitoring_host_ip: 'foo.com'`|
|**config_dir**|Directory for storing configuration files and running docker-compose.yml Use defaults or set up needed value in */vars/main.yml:* <br> <br> `config_dir:  "/path/to/configs"`|
|**cvm_endpoints**| Endpoints for monitoring health checks on ComputeVirtualMachine (CVM) environment. Use defaults or set up needed value in */vars/main.yml:*  <br> <br> `cvm_endpoints:` <br>  `- "https://somename.com/grok_compute/info"`<br> `- "https://cvm.somename.com/jupyter/helper/info"`<br> `- "http://ipaddress/notebook/helper/info"`| 
|**datagrok_endpoints**|Endpoints for monitoring health checks on Datagrok environment. Use defaults or set up needed value *in /vars/main.yml:* <br><br>  `datagrok_endpoints:` <br>   `-  "https://somename.com/api/admin/health"`<br> `-  "https://test.somename.com/api/admin/health"`|
|**deploy_profile**|Profile for deploying monitoring service. Accepting values: <br><br>`- all`<br> `- grafana`<br> `- alertmanager`<br> `- blackbox`<br><br> There are one or several values that can be used. [Prometheus ]( https://prometheus.io/docs/introduction/overview/ ) will be deployed anyway. Other services will be deployed if their names are present in **deploy_profile** variable value. `all` - deploy all services|
|**prometheus_static_targets**|Targets for [Node Exporter](https://prometheus.io/docs/guides/node-exporter/) to monitor hardware node parameters Use defaults or set up needed value in */vars/main.yml:* <br><br>  `prometheus_static_targets:` <br> `- '44.55.66.123'`<br> `- 'some.com' `|
|**cadvisor_port**|External port for  [ Cadvisor exporter ]( https://github.com/google/cadvisor ) .  [ Prometheus ]( https://prometheus.io/docs/introduction/overview/ )  will scrape metrics from here: <br> `http(s)://adrdress:cadvisor_port`|
|**prometheus_port**|External port for [ Prometheus ]( https://prometheus.io/docs/introduction/overview/ ). Used for checking [ Prometheus ]( https://prometheus.io/docs/introduction/overview/ )|
|**grafana_port**|External port for [ Grafana ]( https://grafana.com/grafana/ ). Use `http(s)://address.com:grafana_port value` to enter [Grafana](https://grafana.com/grafana/).|
|**alertmanager_port**|External port for [ Alert Manager ]( https://prometheus.io/docs/alerting/latest/alertmanager/ ). Use  `http(s)://address.com:alertmanager_port value`  to enter  [Alert Manager](https://prometheus.io/docs/alerting/latest/alertmanager/).|
|**blackbox_port**| External port for [ BlackBox Exporter ]( https://github.com/prometheus/blackbox_exporter ). Use  `http(s)://address.com:blackbox_port value`  to enter  [BlackBox Exporter](https://github.com/prometheus/blackbox_exporter)|
|**smtp_port**|Mail sending port. Defaul tvalue is `25`|
|**smtp_login**|Login to SMTP mail server. Example: `name@mail.some`|
|**smtp_pwd**|Password to SMTP mail server.|
|**note_receiver**|E-mail for alert receiving. Can have several values. <br><br> `note_receiver:`<br> `-a@sd.com` <br> `-b@asd.com`|
|**sending_host**|SMTP mail server IP address or DNS address|


Example Playbook
----------------


    - hosts: servers
      roles:

         - role: monitoring
           vars:
             monitoring_grafana_enabled: false
             note_receiver:
               -a@sd.com
               -b@asd.com
             monitoring_server_inventory_group: server
             monitoring_targets_inventory_group: targets


 
=======
  
License
-------

Datagrok

Author Information
------------------

Dmytro Nahovskyi,
E-mail: *dnahovskyi@datagrok.ai*