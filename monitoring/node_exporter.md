1. Downloading and installing Node Exporter on the server:

```bash
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
tar xvf node_exporter-1.8.2.linux-amd64.tar.gz
sudo mv node_exporter-1.8.2.linux-amd64/node_exporter /usr/local/bin/
```

2. Creating a systemd service file for Node Exporter:

```bash

[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target

```

3. Creating a user for Node Exporter and starting the service:

```bash
sudo useradd --no-create-home --shell /usr/sbin/nologin node_exporter
sudo systemctl daemon-reload
sudo systemctl enable --now node_exporter
```

4. Testing Node Exporter by accessing the metrics endpoint and checking the service status:

```bash
curl localhost:9100/metrics
sudo systemctl status node_exporter
```
