# Moving windows recovery partition

You may say to youself, "I can always roll back to my last good snapshot?" 
If you like myself are running Windows within a VM, backed by a ZVolume, this should be trivial, right?
While this is true, this is sometimes overkill.
Often windows is failing due to something stupid it did to itself and unless it knows to rollback that change, it may happen again.

## Minor Note

By default Windows 11 uses bitlock encryption.
If you're using ZFS/ZVOL (to store VM images) it is probably wise not to do any of this until you disable bitlock encryption.
As modern windows does disk encryption by default.
Run encryption on your ZFS Pool not within your VMs.

## Procedure

* Back up your system
  1. Reboot you system,  ensure this reboot is clean (no errors)
  2. Shutdown your system.
  3. Backup your system while offline (avoid ram snapshop headaches).
* Launch terminal
  1. windows key, type "cmd", right click on "Command Prompt" select "Run as administrator"
  2. Within the terminal type, `reagentc /disable`
  3. Now recovery is disabled, welcome to the danger zone.
  4. Run `diskpart`

You should now see something very much like

```
C:\Windows\System32>diskpart

Microsoft DiskPart version XX.XXX.XXXXXXX.XXXXXXX

Copyright (C) Micro$oft Corporation.
On computer: Compy 386

DISKPART>
```

Run the command `list disk`

```
C:\Windows\System32>diskpart

Microsoft DiskPart version XX.XXX.XXXXXXX.XXXXXXX

Copyright (C) Micro$oft Corporation.
On computer: Compy 386

DISKPART> list disk

 Disk #### Status     Size    Free    Dyn Gpt
 --------- ---------- ------- ------- --- ---
 Disk 0    Online     1006GB    750GB       *
```

Good, we have free space!

We will select the first disk with `select disk 0`

```
DISKPART> list disk

 Disk #### Status     Size    Free    Dyn Gpt
 --------- ---------- ------- ------- --- ---
 Disk 0    Online     1006GB    750GB       *

DISKPART> select disk 0

Disk 0 is now the selected disk
```

List the partitions, ideally you should see some free space.

```
DISKPART> select disk 0

Disk 0 is now the selected disk

DISKPART> list partition

Partition #### Type             Size    Offset
-------------- ---------------- ------- -------
Partition 1    System             100MB  1024KB
Partition 2    Reserved            16MB   101MB
Partition 3    Primary            255GB   117MB
Partition 4    Recovery           642MB   255GB
```

Select the partition that is the `Recovery` partition, in this case it is `4`
Use the command `select partition #` to select the partition.
Use the command `delete partition override` to delete it

```
DISKPART> list partition

Partition #### Type             Size    Offset
-------------- ---------------- ------- -------
Partition 1    System             100MB  1024KB
Partition 2    Reserved            16MB   101MB
Partition 3    Primary            255GB   117MB
Partition 4    Recovery           642MB   255GB

DISKPART> select partition 4

Partition 4 is now the selected partition

DISKPART> delete partition override

DiskPart successfully deleted the selected partition.

DISKPART> list partition

Partition #### Type             Size    Offset
-------------- ---------------- ------- -------
Partition 1    System             100MB  1024KB
Partition 2    Reserved            16MB   101MB
Partition 3    Primary            255GB   117MB
```

Now we can use the standard disk manage UI within Windows10/11 to resize our partition.
You'll want to leave ~1GB of free space.
After performing these actions (within the disk manager graphical interface) you'll see something like

```
DISKPART> list partition

Partition #### Type             Size     Offset
-------------- ---------------- -------  -------
Partition 1    System             100MB   1024KB
Partition 2    Reserved            16MB    101MB
Partition 3    Primary            1005GB   117MB
Partition 4    Primary             900MB  1005GB
```

We now once again select that small partition at the end with `select partition 4` (in our case)

```
DISKPART> list partition

Partition #### Type             Size     Offset
-------------- ---------------- -------  -------
Partition 1    System             100MB   1024KB
Partition 2    Reserved            16MB    101MB
Partition 3    Primary            1005GB   117MB
Partition 4    Primary             900MB  1005GB

DISKPART> select partition 4

Partition 4 is now the selected partition
```

Now we assign a magic number to partition, so windows knows this is a recovery partition.
We do this with the command `set id=de94bba4-06d1-4d40-a16a-bfd50179d6ac`
We can then check out changes too effect by listing partitions

```

DISKPART> select partition 4

Partition 4 is now the selected partition

DISKPART> set id=de94bba4-06d1-4d40-a16a-bfd50179d6ac

DiskPart successfully set the partition ID.

DISKPART> list partition

Partition #### Type             Size     Offset
-------------- ---------------- -------  -------
Partition 1    System             100MB   1024KB
Partition 2    Reserved            16MB    101MB
Partition 3    Primary            1005GB   117MB
Partition 4    Recovery            900MB  1005GB
```

Now we should only need to re-enable recovery.

Leave diskpart with `exit` and run the command `reagentc /enable`

```
DISKPART> exit

Leaving DiskPart...

C:\Windows\System32> reagentc /enable
REAGENTC.EXE: Operation Successful.

C:\Windows\System32>
```

That should be everything!
