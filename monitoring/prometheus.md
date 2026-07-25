1. Downloading and installing Prometheus on the server:

```bash
cd /tmp
wget https://github.com/prometheus/prometheus/releases/download/v2.53.0/prometheus-2.53.0.linux-amd64.tar.gz
tar xvf prometheus-2.53.0.linux-amd64.tar.gz
```

2. Creating a Prometheus user and necessary directories so that Prometheus can run as a non-root user:

```bash
sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
sudo mkdir /etc/prometheus /var/lib/prometheus
```

3. Moving Prometheus binaries and configuration files to appropriate directories:

```bash
cd prometheus-2.53.0.linux-amd64
sudo mv prometheus promtool /usr/local/bin/
sudo mv consoles console_libraries /etc/prometheus/
```

4. Creating a Prometheus configuration file:

```bash
sudo nano /etc/prometheus/prometheus.yml
```

Inside the file, adding the following configuration:

```bash
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']
```

These configurations set the global scrape interval to 15 seconds and define a scrape job for the Node Exporter running on localhost at port 9100.

5. Setting the ownership of Prometheus files and directories to the Prometheus user:

```bash
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus /usr/local/bin/prometheus /usr/local/bin/promtool
```

6. Creating a systemd service file for Prometheus:

```bash
sudo nano /etc/systemd/system/prometheus.service
```

Inside the file, adding the following configuration:

```bash
[Unit]
Description=Prometheus
After=network.target

[Service]
User=prometheus
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/ \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

It denotes that Prometheus will run as the prometheus user, using the specified configuration file and storage path by default.

7. Reloading the systemd daemon, enabling, and starting the Prometheus service:


```bash
sudo systemctl daemon-reload
sudo systemctl enable --now prometheus
```

8. Testing Prometheus by checking the service status and accessing the Prometheus web interface:

```bash
sudo systemctl status prometheus
```

The Prometheus web interface can be accessed by navigating to the following URL in a web browser as port 9090 is the default port fixed for Prometheus:

http://<vm-ip>:9090