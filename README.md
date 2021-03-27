```
$ ls -trl
total 67930144
-rw-------  1 hisayuki  staff  15851323392  3  8 16:47 ubuntu20_2.vdi
drwx------  6 hisayuki  staff          192  3 27 18:26 Logs
-rw-------  1 hisayuki  staff  18926796800  3 27 21:49 ubuntu20.vdi
-rw-------  1 hisayuki  staff         7671  3 27 21:49 ubuntu20.vbox-prev
-rw-------  1 hisayuki  staff         4488  3 27 21:49 ubuntu20.vbox
-rw-------  1 hisayuki  staff      2097152  3 27 21:54 ubuntu20_30G.vdi

$ VBoxManage clonehd --existing "ubuntu20.vdi" "ubuntu20_30G.vdi" 
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
Clone medium created in format 'VDI'. UUID: f08c1253-9496-4a4a-adf5-20de40af49a5

$ ssh -p 2022 xxxxx@127.0.0.1
Welcome to Ubuntu 20.04 LTS (GNU/Linux 5.4.0-60-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat 27 Mar 2021 01:09:08 PM UTC

  System load:                      1.16
  Usage of /:                       90.4% of 19.56GB
  Memory usage:                     7%
  Swap usage:                       0%
  Processes:                        138
  Users logged in:                  0
  IPv4 address for br-cfe80cfc6497: 192.168.49.1
  IPv4 address for docker0:         172.17.0.1
  IPv4 address for enp0s3:          10.0.2.15
  IPv4 address for enp0s8:          192.168.0.1

  => / is using 90.4% of 19.56GB

 * Introducing self-healing high availability clusters in MicroK8s.
   Simple, hardened, Kubernetes for production, from RaspberryPi to DC.

     https://microk8s.io/high-availability

176 updates can be installed immediately.
46 of these updates are security updates.
To see these additional updates run: apt list --upgradable


Last login: Sat Mar 27 10:48:39 2021 from 10.0.2.2

$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
udev             1971404        0   1971404   0% /dev
tmpfs             403068     1096    401972   1% /run
/dev/sda2       20508240 18537812    905624  96% /
tmpfs            2015328        0   2015328   0% /dev/shm
tmpfs               5120        0      5120   0% /run/lock
tmpfs            2015328        0   2015328   0% /sys/fs/cgroup
/dev/loop1        101632   101632         0 100% /snap/core/10859
/dev/loop2         33152    33152         0 100% /snap/snapd/11402
/dev/loop3         56832    56832         0 100% /snap/core18/1944
/dev/loop0        100736   100736         0 100% /snap/core/10823
/dev/loop4        134784   134784         0 100% /snap/docker/796
/dev/loop5         56832    56832         0 100% /snap/core18/1988
/dev/loop6         70400    70400         0 100% /snap/lxd/19823
/dev/loop7         73216    73216         0 100% /snap/lxd/19566
/dev/loop8        128896   128896         0 100% /snap/docker/471
/dev/loop9         33152    33152         0 100% /snap/snapd/11107
tmpfs             403064        0    403064   0% /run/user/1000

$ sudo parted
GNU Parted 3.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) print
Warning: Not all of the space available to /dev/sda appears to be used, you can
fix the GPT to use all of the space (an extra 20971520 blocks) or continue with
the current setting? 
Fix/Ignore? Fix                                                           
Model: ATA VBOX HARDDISK (scsi)
Disk /dev/sda: 32.2GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name  Flags
 1      1049kB  2097kB  1049kB                     bios_grub
 2      2097kB  21.5GB  21.5GB  ext4

(parted) resizepart 2                                                     
Warning: Partition /dev/sda2 is being used. Are you sure you want to continue?
Yes/No? Yes                                                               
End?  [21.5GB]? 32GB                                                      
(parted) print                                                            
Model: ATA VBOX HARDDISK (scsi)
Disk /dev/sda: 32.2GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name  Flags
 1      1049kB  2097kB  1049kB                     bios_grub
 2      2097kB  32.0GB  32.0GB  ext4

(parted) q                                                                
Information: You may need to update /etc/fstab.

$ sudo resize2fs /dev/sda2
resize2fs 1.45.5 (07-Jan-2020)
Filesystem at /dev/sda2 is mounted on /; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 4
The filesystem on /dev/sda2 is now 7811988 (4k) blocks long.

$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
udev             1971404        0   1971404   0% /dev
tmpfs             403068     1096    401972   1% /run
/dev/sda2       30625276 18556176  10593248  64% /
tmpfs            2015328        0   2015328   0% /dev/shm
tmpfs               5120        0      5120   0% /run/lock
tmpfs            2015328        0   2015328   0% /sys/fs/cgroup
/dev/loop1        101632   101632         0 100% /snap/core/10859
/dev/loop2         33152    33152         0 100% /snap/snapd/11402
/dev/loop3         56832    56832         0 100% /snap/core18/1944
/dev/loop0        100736   100736         0 100% /snap/core/10823
/dev/loop4        134784   134784         0 100% /snap/docker/796
/dev/loop5         56832    56832         0 100% /snap/core18/1988
/dev/loop6         70400    70400         0 100% /snap/lxd/19823
/dev/loop7         73216    73216         0 100% /snap/lxd/19566
/dev/loop8        128896   128896         0 100% /snap/docker/471
/dev/loop9         33152    33152         0 100% /snap/snapd/11107
tmpfs             403064        0    403064   0% /run/user/1000
```
