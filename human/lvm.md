#############################################
# enlarge lvm parition with additional disk
#############################################
# see disks
lsblk

#1) Run fdisk pour pouvoir gérer les disk 
fdisk /dev/sdx

#2) Creation d'une partition de type "Linux LVM" = 8e
m-n-p- -t -8e - w

#3) Initialize a disk or PARTITION for use by LVM
pvcreate /dev/sdxX

#4) Add physical volumes to a volume group (/dev/sdb1)
vgdisplay /dev/sdaX
vgextend system-vg /dev/sdbx

#6) Extend the size of a logical volume
lvextend -l +100%FREE /dev/vg_data/lv_data

#7) 
resize2fs /dev/VG_Name/LV_Name
# notice that lvm create a directory with vg name and subfile with lv name
# example :
resize2fs /dev/HOSTNAME-vg/root
resize2fs /dev/system-vg/root


#8) to check if the procedure has been succefully succeed don't use 'lsblk' BUT 'df -h' instead
df -h 

#############################################
Mount new disk on /data
#############################################
mkdir /data

# pvcreate
pvcreate /dev/sdb
pvdisplay

# vgcreate
vgcreate vg_NAME /dev/sdb
vgdisplay

# lvcreate
lvcreate -l +100%FREE -n lv_NAME vg_NAME

# file system create
# mkfs.extX
mkfs.ext4 /dev/mapper/vg_NAME-lv_NAME

# mount the Logicial Volume in /data
mount /dev/mapper/vg_NAME-lv_NAME /data

# rendre persistent le mount au demarrage
vim /etc/fstab
copy the first line, and replace with LV path

###############
Delete partition in a DISK
cfdisk /dev/sdb
###############

### reduce lv size
https://www.rootusers.com/lvm-resize-how-to-decrease-an-lvm-partition/