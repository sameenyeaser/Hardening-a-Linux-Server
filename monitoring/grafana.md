1. Installing Grafana on the server:

```bash
sudo apt install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```

2. Adding the Grafana APT repository to the system:

```bash
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```

3. Updating the package list and installing Grafana:

```bash
sudo apt update
sudo apt install grafana -y
``` 

4. Starting and enabling the Grafana service:

```bash
sudo systemctl enable --now grafana-server
```

5. Checking the status of the Grafana service:

```bash
sudo systemctl status grafana-server
```

Grafana should now be running and accessible via a web browser at the following URL as port 3000 was opened in the firewall:

http://<vm-ip>:3000

6. In the Grafana UI, prometheus was added as a data source setting url to 'http://prometheus:9090' 

7. Dashboards were created to visualize the metrics collected by Prometheus from Node Exporter using ID 1860 which is a pre-built dashboard for monitoring Linux systems (well known "Node Exporter Full" dashboard).

8. Testing Grafana by generating some load on the server to see the metrics in action:

```bash
yes > /dev/null &
```

This command generates CPU load on the server, which can be observed in the Grafana dashboards. After testing, stopping the load generation by killing the background process:

```bash
kill %1
```

When running multiple load tests, the 'kill' command may need to be executed with the appropriate job number to stop the correct background process. 
