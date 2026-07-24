
1. Generating a new public and private key pair on the host

```bash
ssh-keygen -t ed25519
```

Default location is accepted with no paraphrase

2. Logging into the VM with the password to ensure it works:

```bash
ssh your-user@vm-ip-address
```

3. Copying the public key to the VM:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub your-user@vm-ip-address
```

4. Editing the SSH configuration file on the VM:

```bash
sudo nano /etc/ssh/sshd_config
```

Setting these lines and removing the comment symbol (#) if present:

```conf
PasswordAuthentication no
PermitRootLogin no
```

Checking the configuration file for syntax errors:

```bash
sudo sshd -t
sudo sshd -T | grep -Ei 'passwordauthentication|PermitRootLogin' 
```

There was an '50-cloud-init.conf' file in the /etc/ssh/sshd_config.d directory that was overriding the main sshd_config file. I had to delete it to make my changes take effect.

5. Restarting the SSH service on the VM:

```bash
sudo systemctl restart ssh
```


6. Testing the new configuration by logging in with the key from the host:

```bash
ssh your-user@vm-ip-address
```

Then trying to force password authentication from the host:

```bash
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no your-user@vm-ip-address
```
More commands used in the process:

- Checking SSH status:

```bash
sudo systemctl status ssh
```

- Reviewing SSH logs:

```bash
sudo journalctl -u ssh -n 50 --no-pager
```

- Confirming the authorized key exists:

```bash
ls -la ~/.ssh
cat ~/.ssh/authorized_keys
```
