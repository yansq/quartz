## list disks on the system

`fdisk -l`

## create partitions

`fdisk /dev/vdb` (replace `/dev/vdb` with your disk name)

then enter commands as the leading, which `np1` means create one partition for the whole disk.

## write file system

`mkfs.ext4 /dev/vdb`

## mount

`mount /dev/vdb /data`

## auto-mount when power on 

`vim /etc/fstab`

add: 
`/dev/vdb /data ext4 defaults 0 0`