1. A new virtual disk was added in the virtualbox and after that the vm was restarted. 

2. Identifying the new disk in the vm using the following command: 

```bash
lsblk
```

The new disk was detected as /dev/sdb.

3. Creating a new physical volume on the new disk:

```bash
sudo pvcreate /dev/sdb
```

4. Creating a new volume group named 'data-vg' using the new physical volume:

```bash
sudo vgcreate data-vg /dev/sdb
``` 

5. Creating a new logical volume named 'data-lv' with a size of 4G in the 'data-vg' volume group:

```bash
sudo lvcreate -L 4G -n data-lv data-vg
``` 

'lvcreate -L 5G' resulted in a failure because the volume group did not have enough free extents for 5G, so 4G was the largest safe size at the moment.

6. Formatting the new logical volume with the ext4 filesystem:

```bash
sudo mkfs.ext4 /dev/data-vg/data-lv
```

7. Creating a mount point for the new logical volume and mounting it:

```bash
sudo mkdir /mnt/data
sudo mount /dev/data-vg/data-lv /mnt/data
``` 

8. Making the mount persistent across reboots by adding an entry to the /etc/fstab file:

```bash
echo '/dev/data-vg/data-lv /mnt/data ext4 defaults 0 2' | sudo tee -a /etc/fstab
```

Testing:

```bash
df -h /mnt/data
```

```bash
sudo touch /mnt/data/testfile
ls -l /mnt/data
```
This will prove that the new logical volume is mounted and writable if it is still there after a reboot.