## Prometheus: starting the server with Alertmanager, cAdvisor and Grafana on Ubuntu

###  Nginx, Docker, Docker-compose installation
```
apt install -y nginx curl vim git

curl https://get.docker.com/ | bash

curl -o /usr/local/bin/docker-compose -L "https://github.com/docker/compose/releases/download/1.17.1/docker-compose-$(uname -s)-$(uname -m)"
chmod +x /usr/local/bin/docker-compose
```

### Configure hosts on host machine
```
sudo sh -c "echo '10.11.100.166 prometheus-server.local' >> /etc/hosts"
```

### Create needed folders and copy configs
```
mkdir -p /data/monitoring/{prometheus-server,grafana}
mkdir /etc/prometheus
mkdir /etc/alertmanager

git pull https://github.com/iliusa77/prometheus-grafana-docker-cadvisor.git
cd prometheus-grafana-docker-cadvisor
cp configs/etc/nginx/conf.d/* /etc/nginx/conf.d/
cp configs/etc/prometheus/* /etc/prometheus/
cp configs/etc/alertmanager/* /etc/alertmanager/
```

### Run Docker stack
```
docker-compose -f prometheus-server.yml up  -d
```

### Prometheus, Grafana, cAdvisor urls
```
# prometheus
http://prometheus-server.local/prometheus/targets
http://prometheus-server.local/prometheus/metrics
http://prometheus-server.local/prometheus/alerts
# grafana
http://prometheus-server.local
# cadvisor
http://prometheus-server.local:8080/metrics
```

### Add Grafana dashboard 
```
http://prometheus-server.local/dashboard/import
in field "Grafana.com Dashboard" fill https://grafana.com/grafana/dashboards/159
```

### Check mailbox alerts-mailbox@gmail.com for notification
```
[1] Firing
Labels
alertname = high_load
instance = node-exporter:9100
job = prometheus-server
monitor = monitoring
name = node-exporter
severity = page
Annotations
description = node-exporter:9100 of job prometheus-server is under high load.
summary = Instance node-exporter:9100 under high load
```
