# Avail Monitoring

The purpose of this repo is to assist validators or inidividuals who are running Avail Nodes that is part of the Polygon ecosystem 


## Install Software

Create the users needed
```
sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
sudo useradd --no-create-home --shell /usr/sbin/nologin node_exporter
```

Get the software needed and Extract
```
cd ~
wget https://github.com/prometheus/prometheus/releases/download/v2.26.0/prometheus-2.26.0.linux-amd64.tar.gz
tar xfz prometheus-*.tar.gz
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz 
tar xvf node_exporter-*.tar.gz
```

Place the software in the correct place
```
cd ~
cd prometheus-*.linux-amd64
sudo cp ./prometheus /usr/local/bin/
sudo cp ./promtool /usr/local/bin/
cd ~
sudo cp ./node_exporter-*.linux-amd64/node_exporter /usr/local/bin/ 
```

Create additional directories needed
```
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

Set the permissions
```
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

Copy the consoles and console_libraries directories to /etc/prometheus, also update permissions
```
cd ~
cd prometheus-*.linux-amd64
sudo cp -r ./consoles /etc/prometheus
sudo cp -r ./console_libraries /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```

Now clean up
```
cd .. && rm -rf prometheus*
rm -rf node_exporter*
```

## Configure prometheus

Edit the configuration file
```
sudo nano /etc/prometheus/prometheus.yml
```

Add the below in the file
```
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: "prometheus"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "substrate_node"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9615"]
  - job_name: node
    static_configs:
      - targets: ['localhost:9100']
```

