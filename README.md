                                           LVM DOCUMENTATION
                                           
                                           
                                           if you are running out of disk space on your server, you can just add another disk and extend the logical volume on the fly.
                                           LVM is a volume manager.
It lets you break up partitions into volumes which makes them easier to manage. You can resize them and mount or dismount them while the system is running. Also, LVM allows for taking snapshots for backups.

Once you connect you hard drive you need to tell them that are going to be part of Volune Group by assigning tag to them. So i have this physical drive which is /dev/sda6 and /dev/sda7
# fdisk /dev/sda6
You will be asked for option in next line so press
:t hit enter
:8e hit enter(this tell the hardrive that you are now a part of LVM)
Repeat it for /dev/sda7 as well
2. Initialize Physical volume
#pvcreate /dev/sda6 /dev/sda7
To view if the drive has been initialized
#pvdisplay

3. To create Volume Group
I want to create a volume group name Bigdata
#vgcreate bigdata /dev/sda6 /dev/sda7
#vgdisplay

4. To create LV from VG
#lvcreate -L 1200M n bigdatalv1 bigdata
-L : it is option for providing size
1200M is the size in Mb
n: is for naming your logical volume, here i have named it bigdatalv1 followed by the VG name.

5. To format Logical Volume
#mkfs.ext4 /dev/bidata/bigdatalv1
6. To mount LV so that it can accesibile, we need to create a directory and mount on it
#mkdir /bigdatadir
#mount /dev/bidata/bigdatalv1 /bigdatadir

7. To extend VG if we have new hard drive to extend follow step1 and 2 then below cmd
#vgextend bigdata /dev/sda8

8. To reduce VG if we want to remove a hard drive from VG
#vgreduce bigdata /dev/sda7

9. To extend LV
#lvextend -L +100M /dev/bigdata/bigdatalv1

10. To reduce LV
#lvreduce -L -100M /dev/bigdata/bigdatalv1


