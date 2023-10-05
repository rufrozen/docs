## sshfs
```
apt install sshfs
mkdir /mnt/filesv
sshfs -o allow_other,reconnect,uid=1000,gid=1000,IdentityFile=/root/.ssh/id_rsa root@10.10.10.10:/opt/data/ /mnt/filesv
```

## raid0
```
# create device
mdadm --create --verbose /dev/md0 --level=0 --raid-devices=5 /dev/vdb /dev/vdc /dev/vdd /dev/vde /dev/vdf

# format to ext4
mkfs.ext4 -F /dev/md0

# add line to /etc/fstab
/dev/md0 /opt ext4 defaults,nofail,discard,noatime 0 0

# apply fstab
mount -a

# list discs
lsblk
```

#### Restore
```
mdadm --stop /dev/md127
mdadm --assemble --scan --verbose
```

## partition
```
# create partition table
parted -l
parted /dev/vdb
> mklabel gpt
> mkpart primary ext4 1 -1

# format
mkfs.ext4 /dev/vdb1

# add to fstab
echo -e "UUID=`blkid -s UUID -o value /dev/vdb1` /home ext4 noatime 0 0" >> /etc/fstab
```

## fstab
```
echo -e "UUID=`blkid -s UUID -o value /dev/vdb2` /home ext4 noatime 0 0" >> /etc/fstab
```

#### bind
```
# copy folders to big disk
rsync -zahP /home/ /data/home/
rsync -zahP /tmp/ /data/tmp/
rsync -zahP /var/ /data/var/
rsync -zahP /opt/ /data/opt/

# add bind to /etc/fstab
vi /etc/fstab
# put it after device mount line in /etc/fstab
/data/home /home none defaults,bind 0 0
/data/tmp /tmp none defaults,bind 0 0
/data/var /var none defaults,bind 0 0
/data/opt /opt none defaults,bind 0 0

# apply
mount -a
```

## swap
```
fallocate -l 16G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo "/swapfile swap swap defaults 0 0" >> /etc/fstab
```

```
mkswap /dev/vdb2
echo -e "UUID=`blkid -s UUID -o value /dev/vdb2` none swap sw 0 0" >> /etc/fstab
```
