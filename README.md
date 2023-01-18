Role Name
=========
Monitoring

Install on one server next monitoring tools:  
* [Prometheus](https://prometheus.io/docs/introduction/overview/)
* [Prometheus Node Exporter](https://github.com/prometheus/node_exporter)
* [Grafana](https://grafana.com/grafana/)
* [Prometheus Alert Manager](https://prometheus.io/docs/alerting/latest/alertmanager/)
* [BlackBox Exporter](https://github.com/prometheus/blackbox_exporter)
* [PostgreSql Exporter](https://github.com/prometheus-community/postgres_exporter)

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
| **monitoring_docker_network_name** | monitoring | Name for monitoring docker network |
| **monitoring_docker_network_subnet** | 172.20.0.0/16 | Subnet address for monitoring docker network |
| **monitoring_config_dir** | /etc/monitoring | Directory for storing <br> configuration files and running docker-compose.yml |
| **monitoring_keep_prometheus_logs** | 200h | Keeping Prometheus logs time interval |
| **monitoring_domain** | 127.0.0.1 | Domain for Prometheus service hosting |
| **monitoring_alertmanager_domain** | `http://{{ monitoring_domain }}:{{ monitoring_alertmanager_port }}` | Alertmanager web path |
| **monitoring_cvm_endpoint** | `http://127.0.0.1:8090` | [CVM](https://datagrok.ai/help/develop/admin/infrastructure#compute-components) server DNS <br> name or IP address, like <br> `https://somename.com`<br> or `http://1.2.3.4.` |
| **monitoring_datagrok_endpoint** | `http://127.0.0.1:8080` | [Datagrok](https://datagrok.ai/help/develop/admin/infrastructure#datagrok-components) server DNS <br>name or IP address like <br>`https://somename.com`<br> or `http://1.2.3.4.` |
|**monitoring_blackbox_endpoints**| `- "{{ monitoring_cvm_endpoint }}/grok_compute/info"`<br> `- "{{ monitoring_cvm_endpoint }}/jupyter/helper/info"`<br> `- "{{ monitoring_cvm_endpoint }}/notebook/helper/info"` <br> `- "{{ monitoring_cvm_endpoint }}/notebook/api"` <br>`- "{{ monitoring_cvm_endpoint }}/jupyter/api/swagger.yaml"` <br> `- "{{ monitoring_cvm_endpoint }}:5005/helper/info"` <br> `- "{{ monitoring_cvm_endpoint }}:54321/3/About"` <br> `- "{{ monitoring_datagrok_endpoint }}/api/admin/health"` | Endpoints for monitoring <br> health checks on [CVM](https://datagrok.ai/help/develop/admin/infrastructure#compute-components)<br> and [Datagrok](https://datagrok.ai/help/develop/admin/infrastructure#datagrok-components) environment.<br> Use defaults or set up <br>needed value | 
|**monitoring_grafana_enabled**| true | Enable option to install<br> [ Grafana ]( https://grafana.com/grafana/ ). Describes <br>monitoring statistics in<br> graph mode. Can get <br>`true` or `false` value. |
|**monitoring_alertmanager_enabled**| true | Enable option to install <br>[Alert Manager](https://prometheus.io/docs/alerting/latest/alertmanager/). Used to <br>send monitoring alerts via e-mail and slack. Can get `true` or `false` value |
|**monitoring_blackbox_enabled**| true | Enable option to install<br> [ BlackBox Exporter ]( https://github.com/prometheus/blackbox_exporter ). Used for monitoring platform healthchecks |
|**monitoring_cadvisor_enabled**| true | Enable option to install <br>[ Cadvisor exporter ]( https://github.com/google/cadvisor ). Used for monitoring docker containers |
| **monitoring_pgsql_exporter_enabled** | true | Enable option to install <br>[PostgreSql Exporter](https://github.com/prometheus-community/postgres_exporter). Used for monitoring PostgreSql databases |
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
|**monitoring_pgsql_exporter_port**|9187|External port for [PostgreSql Exporter](https://github.com/prometheus-community/postgres_exporter)|
|**monitoring_alerts_repeat_message_interval**|30m|Repeat alert message time interval <br> if alert still exists|
|**monitoring_alerts_group_message_interval**|5m|Time interval to send alert message <br> about new alerts that are added <br> to a group for which an initial <br> notification has already been sent|
|**monitoring_alerts_send_resolved**|true|Notify about resolved alerts|
|**monitoring_slack_alerts**|false| Enable sending monitoring alerts via slack |
|**monitoring_slack_channel**|monitoring| Name of slack channel for sending alerts |
|**monitoring_slack_webhook_url**| `" "` | URL for using your Slack|
|**monitoring_email_alerts**|true| Enable sending monitoring alerts via <br> e-mail |
|**monitoring_smtp_host**|127.0.0.1|The SMTP host through which emails <br> are sent|
|**monitoring_smtp_port**|25|Mail sending port|
|**monitoring_smtp_from**|`alertmanager@{{ inventory_hostname }}`|The default SMTP From header field|
|**monitoring_smtp_login**| `""` |Login to SMTP mail server. Example: `name@mail.some`|
|**monitoring_smtp_pwd**| `""` |Password to SMTP mail server.|
|**monitoring_note_receiver**| `monitoring_note_receiver:`<br> `- 'monitoring@datagrok.ai'`|E-mail for alert receiving. Can have several values. |
|**monitoring_smtp_require_tls**| true |Enable tls for alert messages|
|**monitoring_pgsql_db_host**|datagrok|Monitored database host|
|**monitoring_pgsql_dbs**|`- login: 'postgres'`<br>&emsp;&nbsp;`password: 'postgres'`<br>&emsp;&nbsp;`host: '127.0.0.1'`<br>&emsp;&nbsp;`port: 5432`<br>&emsp;&nbsp;`dbname: 'datagrok'`<br>&emsp;&nbsp;`sslmode: 'disable'` |Monitored database credentials|
|**monitoring_pgsql_parameters**|DATA_SOURCE_NAME: [value](https://github.com/datagrok-ai/ansible-role-datagrok-monitoring/blob/pgsql/defaults/main.yml#L92)<br>PG_EXPORTER_WEB_LISTEN_ADDRESS:<br>":{{ monitoring_pgsql_exporter_port }}"|Launch parameters for <br> PostgreSql monitoring|


Grafana Dashboards
-------
We recomend to import these  dashboards for metrics vizualization
| Service | Dashboard number |
| -------- | ------------- |
| [Prometheus Node Exporter](https://github.com/prometheus/node_exporter) | [1860](https://grafana.com/grafana/dashboards/1860-node-exporter-full/) |
| [Cadvisor Exporter](https://github.com/google/cadvisor) | [14282](https://grafana.com/grafana/dashboards/14282-cadvisor-exporter/) |
| [Blackbox Exporter](https://github.com/prometheus/blackbox_exporter) | [7587](https://grafana.com/grafana/dashboards/7587-prometheus-blackbox-exporter/) |
| [PostgeSql Exporter](https://github.com/prometheus-community/postgres_exporter) | [6742](https://grafana.com/grafana/dashboards/6742-postgresql-statistics/) |


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


 
  
License
-------
Datagrok

Author Information
------------------

Dmytro Nahovskyi,
E-mail: *dnahovskyi@datagrok.ai*
