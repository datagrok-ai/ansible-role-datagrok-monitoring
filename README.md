Role Name
=========
Monitoring_server

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
         - "monitoring_server"

License
-------

Datagrok

Author Information
------------------

Dmytro Nahovskyi,
E-mail: *dnahovskyi@datagrok.ai*