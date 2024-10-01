on create a virtual machine , create new resource group 
>> create a vpc >> create a new vpc >> add subntets >> create
>> go to networking  >> choose the public subnet and correct resourcee group
>> Disks >> choose the resource group which you chhoose to create an instance >> review +create
>> Go to vm >> seleect the vm >> click on settings to the left >> choose disks >> attach exisiting disks >> choose your disk >> apply

##############################################

Now to mount the disk on terminal
lsblk
df -h
mkfs. <tab> <tab>
mkfs.ext4 /dev/xvdb
mkdir /data
mount /dev/xvdb /data
blkid >copy uuid of xvdb and paste it below 
vim /etc/fstab {uuid = …. /data ext4 defaults 0 0)
df -h
cd /data
touch glenn.txt
mount -a
Reboot the instance and check if the data is mounted or not 
cd /data
ls
df -h
