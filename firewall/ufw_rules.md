1. Setting the default policies for incoming and outgoing connections:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

2. Allowing SSH connections on port 22:

```bash
sudo ufw allow 22
```

Opens port 22 for SSH connections.

3. Allowing HTTP connections on port 3000 for grafana, port 9090 for prometheus, and port 9100 for node exporter:

```bash
sudo ufw allow 3000
sudo ufw allow 9090
sudo ufw allow 9100
```

4. Enabling the UFW firewall:

```bash
sudo ufw enable 
```

5. Checking the status of the UFW firewall:

```bash
sudo ufw status verbose
``` 

Testing in the host machine:

```bash
ssh your-user@vm-ip-address
```

With an invalid port to test the firewall:

```bash
nc -vz vm-ip-address 8080 
```

Expected output: `nc: connect to vm-ip-address port 8080 (tcp) failed: Connection timed out`