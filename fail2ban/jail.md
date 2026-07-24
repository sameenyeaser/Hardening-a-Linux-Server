1. Installing fail2ban on the server:

```bash
sudo apt install fail2ban
```

2. Creating a local configuration file to override the default settings:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

3. Editing the local configuration file to set the ban time, find time, and max retry attempts:

```bash
sudo nano /etc/fail2ban/jail.local
``` 
Inside the file, setting the following values:

[sshd]
enabled = true
port = 22
maxretry = 5
bantime = 600
findtime = 600

4. Restarting the fail2ban service to apply the changes:

```bash
sudo systemctl restart fail2ban
```

5. Checking the status of the fail2ban service:

```bash
sudo systemctl status fail2ban
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

Testing fail2ban:

- Testing the fail2ban configuration by attempting to SSH into the server with incorrect credentials multiple times to trigger the ban. After reaching the max retry attempts, the IP address should be banned for the specified bantime.

```bash
ssh your-user@vm-ip-address
```

on host 

- After the ban is triggered, checking the fail2ban log to confirm the ban in the server:

```bash
sudo tail -f /var/log/fail2ban.log
```

- Unbanning the IP address manually for testing purposes:

```bash
sudo fail2ban-client set sshd unbanip your-ip-address
```

- Testing the unban by attempting to SSH into the server again with the correct credentials:

```bash
ssh your-user@vm-ip-address
```

