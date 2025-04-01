# hyperv_switch_backup
Powershell script to backup / restore virtual switches on a Hyper-V host

# Perform a backup
To backup the configuration of all virtual switches on a Hyper-V host to a JSON file:
```
PS> X:\path\to\hyperv-switch-backup-restore.ps1 -Mode backup
```

# Perform a restore
To restore a previously created backup, use the following syntax.  Please note that any virtual switches that already exist on the Hyper-V host will be skipped.
```
PS> X:\path\to\hyperv-switch-backup-restore.ps1 -Mode restore -FilePath X:\path\to\HyperV_SwitchBackup_yyyyddmm_hhmmss.json
```

# Schedule weekly backups
To automate backups of the Hyper-V virtual switch configs, you can use a scheduled task:
```
schtasks.exe /create /RU SYSTEM /TN hyperv_backup /TR "powershell.exe x:\path\to\hyperv-switch-backup-restore.ps1 -Mode backup" /SC weekly /D FRI /ST 23:00
```

# Sample output

Sample output for a backup job:
```
PS C:\MyScripts> .\HyperV-switch-backup-restore.ps1 -Mode backup
Backup completed: C:\backup\hyperv\vswitch\HYPERV2.vswitch.backup.json
```

Sample output for a restore job:
```
PS C:\MyScripts> .\HyperV-switch-backup-restore.ps1 -Mode restore -FilePath C:\backup\hyperv\vswitch\HYPERV2.vswitch.backup.json 
Switch 'vswitch-external' already exists. Skipping.
Creating switch: vswitch-private1 (Private)
Creating switch: vswitch-private2 (Private)
Creating switch: My internal vswitch 1 (Internal)
Creating switch: my internal vswitch 2 (Internal)
Restore completed from: C:\backup\hyperv\vswitch\HYPERV2.vswitch.backup.json
```

# Send backup report via email
To send an email report showing the backup status, please edit the following section of the script:
```
$send_email = "yes"                                            #yes|no flag to send report via email to the sysadmin
$hostname   = $env:computername                                #figure out the local hostname
$to         = "recipient@example.com"                          #sysadmin that receives email report
$from       = "sender@example.com"                             #from address
$subject    = "Hyper-V vswitch backup report from $hostname"   #subject of email message
$smtpserver = "MySmtpServer.example.com"                       #SMTP smart host used for relaying the email
$port       = "25"                                             #SMTP TCP port
```
