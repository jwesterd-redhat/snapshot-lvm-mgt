# Download from Repo (https example):
```
git clone https://github.com/jwesterd-redhat/snapshot-lvm-mgt.git

```


Review Usage: 

```
[root@jwesterd scripts]# ./snapmgt.bash  --usage

Usage: ./snapmgt.bash
  status - show status of snaps on the system
  createsnaps - create snaps of all LVs in RootVG
  revert - Revert to original OS - merge any snaps back in
  deletesnaps - Accept upgraded OS, delete any point-in-time copies

Export variables for additional snapshot size control
  LVPERCENT    Controls the percentage of the original LVsize (1-115)
  LVMAXMB      Max size in MB for any LV ( 4000 set max snapshot to 4GB)
  Debug	       extra printed hints along the way (export debug true to enable)
```


## Review the skip list, and the percentage and MAXDB usage.

## Create Default snapshots: 

```
[root@jwesterd scripts]# ./snapmgt.bash  createsnaps
Cleanup of /var/lib/leapp
Updating Subscription Management repositories.
63 files removed
Updating Subscription Management repositories.
0 files removed
Setting up /var/lib/leapp -> /var/tmp/leap

Free space check
== Snap volumes to be created at 100% ==
== Snap volumes Max size at 6000MB ==
Removing excluded LVs from the todo list
LVS on local system:  
/dev/RHELCSB/Home
/dev/RHELCSB/Root
/dev/RHELCSB/Swap
/dev/RHELCSB/data
/dev/RHELCSB/virt
Skip List  : 
/var/tmp /opt/abc /var/junk/junk /var/cache /var/lib/leapp
LVs to snap: 
/dev/RHELCSB/data
/dev/RHELCSB/Home
/dev/RHELCSB/Root
/dev/RHELCSB/Swap
/dev/RHELCSB/virt
Saving copy of the files in /boot
=== LV /dev/RHELCSB/data found with size: 255875MB ===
  !! Max LV size for /dev/RHELCSB/data capped at 6000MB !!
  Creating snapshot of /dev/RHELCSB/data with size 6000
  Logical volume "pre_upgrade_data" created.
=== LV /dev/RHELCSB/Home found with size: 112635MB ===
  !! Max LV size for /dev/RHELCSB/Home capped at 6000MB !!
  Creating snapshot of /dev/RHELCSB/Home with size 6000
  Logical volume "pre_upgrade_Home" created.
=== LV /dev/RHELCSB/Root found with size: 51190MB ===
  !! Max LV size for /dev/RHELCSB/Root capped at 6000MB !!
  Creating snapshot of /dev/RHELCSB/Root with size 6000
  Logical volume "pre_upgrade_Root" created.
=== LV /dev/RHELCSB/Swap found with size: 31893MB ===
  !! Max LV size for /dev/RHELCSB/Swap capped at 6000MB !!
  Creating snapshot of /dev/RHELCSB/Swap with size 6000
  Logical volume "pre_upgrade_Swap" created.
=== LV /dev/RHELCSB/virt found with size: 307125MB ===
  !! Max LV size for /dev/RHELCSB/virt capped at 6000MB !!
  Creating snapshot of /dev/RHELCSB/virt with size 6000
  Logical volume "pre_upgrade_virt" created.
```

## Checking status
```
[root@jwesterd scripts]# ./snapmgt.bash  status
LVs in root VG RHELCSB: 
/dev/RHELCSB/Home
/dev/RHELCSB/Root
/dev/RHELCSB/Swap
/dev/RHELCSB/data
/dev/RHELCSB/virt

Active snaps in RHELCSB:
pre_upgrade_Home
pre_upgrade_Root
pre_upgrade_Swap
pre_upgrade_data
pre_upgrade_virt

Status of Existing snaps on host:jwesterd.remote.csb
  LV               VG      Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  pre_upgrade_Home RHELCSB swi-a-s--- <5.86g      Home   0.01                                   
  pre_upgrade_Root RHELCSB swi-a-s--- <5.86g      Root   0.01                                   
  pre_upgrade_Swap RHELCSB swi-a-s--- <5.86g      Swap   0.00                                   
  pre_upgrade_data RHELCSB swi-a-s--- <5.86g      data   0.00                                   
  pre_upgrade_virt RHELCSB swi-a-s--- <5.86g      virt   0.00                                   

Operational status of Root LVs on the system
  Path                          DMPath                               OSize      Data%  Snap% 
  /dev/RHELCSB/Home             /dev/mapper/RHELCSB-Home             112640.00m              
  /dev/RHELCSB/Root             /dev/mapper/RHELCSB-Root              51200.00m              
  /dev/RHELCSB/Swap             /dev/mapper/RHELCSB-Swap               8192.00m              
  /dev/RHELCSB/data             /dev/mapper/RHELCSB-data             256000.00m              
  /dev/RHELCSB/pre_upgrade_Home /dev/mapper/RHELCSB-pre_upgrade_Home 112640.00m 0.01   0.01  
  /dev/RHELCSB/pre_upgrade_Root /dev/mapper/RHELCSB-pre_upgrade_Root  51200.00m 0.01   0.01  
  /dev/RHELCSB/pre_upgrade_Swap /dev/mapper/RHELCSB-pre_upgrade_Swap   8192.00m 0.00   0.00  
  /dev/RHELCSB/pre_upgrade_data /dev/mapper/RHELCSB-pre_upgrade_data 256000.00m 0.00   0.00  
  /dev/RHELCSB/pre_upgrade_virt /dev/mapper/RHELCSB-pre_upgrade_virt 307200.00m 0.00   0.00  
  /dev/RHELCSB/virt             /dev/mapper/RHELCSB-virt             307200.00m              

Storage Consumption of Root LVs on the system
LV /dev/RHELCSB/Home uses 104777 MB
LV /dev/RHELCSB/Root uses 18271 MB
LV /dev/RHELCSB/Swap uses 0 MB
LV /dev/RHELCSB/data uses 1817 MB
LV /dev/RHELCSB/virt uses 272617 MB
Storage used by all LVs: 397482 MB
Checking /boot copy:
We have a copy of the boot directory
```

Delete snapshots: 

```
## Deleting Snapshots: 

[root@jwesterd scripts]# ./snapmgt.bash  deletesnaps
Delete snaps of the OS
  Logical volume "pre_upgrade_Home" successfully removed.
  Logical volume "pre_upgrade_Root" successfully removed.
  Logical volume "pre_upgrade_Swap" successfully removed.
  Logical volume "pre_upgrade_data" successfully removed.
  Logical volume "pre_upgrade_virt" successfully removed.

Check Status after: 

[root@jwesterd scripts]# ./snapmgt.bash  status
LVs in root VG RHELCSB: 
/dev/RHELCSB/Home
/dev/RHELCSB/Root
/dev/RHELCSB/Swap
/dev/RHELCSB/data
/dev/RHELCSB/virt

Active snaps in RHELCSB:

Status of Existing snaps on host:jwesterd.remote.csb

Operational status of Root LVs on the system
  Path              DMPath                   OSize Data%  Snap% 
  /dev/RHELCSB/Home /dev/mapper/RHELCSB-Home                    
  /dev/RHELCSB/Root /dev/mapper/RHELCSB-Root                    
  /dev/RHELCSB/Swap /dev/mapper/RHELCSB-Swap                    
  /dev/RHELCSB/data /dev/mapper/RHELCSB-data                    
  /dev/RHELCSB/virt /dev/mapper/RHELCSB-virt                    

Storage Consumption of Root LVs on the system
LV /dev/RHELCSB/Home uses 104778 MB
LV /dev/RHELCSB/Root uses 18272 MB
LV /dev/RHELCSB/Swap uses 0 MB
LV /dev/RHELCSB/data uses 1817 MB
LV /dev/RHELCSB/virt uses 272617 MB
Storage used by all LVs: 397484 MB
Checking /boot copy:
We have a copy of the boot directory

 Status of first 2 bootable kernels in order: 
kernel="/boot/vmlinuz-4.18.0-477.27.1.el8_8.x86_64"
args="ro vconsole.keymap=us crashkernel=auto rd.lvm.lv=RHELCSB/Root rd.luks.uuid=luks-e9d2d291-2b40-4f08-9e22-e1e955d6284e rhgb quiet rd.luks.key=e9d2d291-2b40-4f08-9e22-e1e955d6284e=/keyfile:UUID=495cd8e1-2b74-4b3c-adfb-f4fdb6535e0e ipv6.disable=1 logo.nologo loglevel=2 $tuned_params"
kernel="/boot/vmlinuz-4.18.0-477.21.1.el8_8.x86_64"
args="ro vconsole.keymap=us crashkernel=auto rd.lvm.lv=RHELCSB/Root rd.luks.uuid=luks-e9d2d291-2b40-4f08-9e22-e1e955d6284e rhgb quiet rd.luks.key=e9d2d291-2b40-4f08-9e22-e1e955d6284e=/keyfile:UUID=495cd8e1-2b74-4b3c-adfb-f4fdb6535e0e ipv6.disable=1 logo.nologo loglevel=2 $tuned_params"

Default kernel to boot from: 
/boot/vmlinuz-4.18.0-477.27.1.el8_8.x86_64
[root@jwesterd scripts]#
```
