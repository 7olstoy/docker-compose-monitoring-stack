docker-compose-monitoring-stack
========

A monitoring solution for Docker hosts and containers with [Prometheus](https://prometheus.io/), [Grafana](http://grafana.org/), [VictoriaMetrics](https://hub.docker.com/r/victoriametrics/victoria-metrics),
[NodeExporter](https://github.com/prometheus/node_exporter), [Blackbox-exporter](https://hub.docker.com/r/prom/blackbox-exporter) and alerting with [AlertManager](https://github.com/prometheus/alertmanager).

## Install

Clone this repository on your Docker host, cd into dockprom directory and run compose up:

```bash
git clone https://github.com/A-styler/docker-compose-monitoring-stack.git
cd docker-compose-monitoring-stack

ADMIN_USER=admin ADMIN_PASSWORD=admin docker-compose up -d
```

Prerequisites:

* Docker Engine >= 1.13
* Docker Compose >= 1.11
* Additional docker network nginx_proxy with any webserver

Containers:

* Prometheus (metrics database) 
* Prometheus-Pushgateway (push acceptor for ephemeral and batch jobs)
* VictoriaMetrics as prom storage
* VMalert for connect to AlertManager
* AlertManager (alerts management)
* Grafana (visualize metrics)
* NodeExporter (host metrics collector)
* Blackbox-exporter for monitoring urls 

## Setup Grafana

Navigate to `http://<host-ip>:3000` and login with user ***admin*** password ***admin***. You can change the credentials in the compose file or by supplying the `ADMIN_USER` and `ADMIN_PASSWORD` environment variables on compose up. The config file can be added directly in grafana part like this
```
grafana:
  image: grafana/grafana:5.2.4
  env_file:
    - config

```
and the config file format should have this content
```
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=changeme
GF_USERS_ALLOW_SIGN_UP=false
```
If you want to change the password, you have to remove this entry, otherwise the change will not take effect
```
- grafana_data:/var/lib/grafana
```

Grafana is preconfigured with dashboards and Prometheus as the default data source:

* Name: VictoriaMetrics
* Type: Prometheus
* Url: http://victoriametrics:8428
* Access: proxy

