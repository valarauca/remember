To find `\Device\RaidPort##` mapping to physical drives when experiencing `stornvme` errors causing windows to crash and/or misbehave you can run 

```powershell
0..3 | ForEach-Object {
    $disk = Get-Disk -Number $_
    $port = Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Enum\SCSI\Disk&Ven_NVMe&Prod_*" | Where {$_.FriendlyName -eq $disk.FriendlyName}
    [PSCustomObject]@{
        RaidPort = "RaidPort$_"
        DiskNumber = $_
        FriendlyName = $disk.FriendlyName
        SerialNumber = $disk.SerialNumber
    }
} | Format-Table
```

This can make `\Device\RaidPort###` into physical numbers. For example

```powershell
PS C:\Users\$snip> 0..3 | ForEach-Object {
>>     $disk = Get-Disk -Number $_
>>     $port = Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Enum\SCSI\Disk&Ven_NVMe&Prod_*" | Where {$_.FriendlyName -eq $disk.FriendlyName}
>>     [PSCustomObject]@{
>>         RaidPort = "RaidPort$_"
>>         DiskNumber = $_
>>         FriendlyName = $disk.FriendlyName
>>         SerialNumber = $disk.SerialNumber
>>     }
>> } | Format-Table

RaidPort  DiskNumber FriendlyName           SerialNumber
--------  ---------- ------------           ------------
RaidPort0          0 WD_BLACK SN850X 4000GB E823_8FA6_BF53_0001_001B_448B_4006_2241.
RaidPort1          1 WD_BLACK SN8100 1000GB E823_8FA6_BF53_0001_001B_448B_4DFA_A8D3.
RaidPort2          2 WD_BLACK SN7100 1TB    E823_8FA6_BF53_0001_001B_448B_4271_09A8.
RaidPort3          3 WD_BLACK SN7100 4TB    E823_8FA6_BF53_0001_001B_448B_4DEF_CB54.
```
