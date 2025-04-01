# hyperv_switch_backup
Powershell script to backup / restore virtual switches on a Hyper-V host

# Perform a backup
To backup the configuration of all virtual switches on a Hyper-V host to a JSON file:
```
X:\path\to\HyperV-switch-backup-restore.ps1 -Mode backup
```

# Perform a restore
To restore a previously created backup, use the following syntax.  Please note that any virtual switches that already exist on the Hyper-V host will be skipped.
```
X:\path\to\HyperV-switch-backup-restore.ps1 -Mode restore -FilePath X:\path\to\HyperV_SwitchBackup_yyyyddmm_hhmmss.json
```

# Schedule weekly backups
To automate backups of the Hyper-V virtual switch configs, you can use a scheduled task:
```
schtasks.exe /create /RU SYSTEM /TN hyperv_backup /TR "powershell.exe x:\path\to\hyperv_switch_backup.ps1 -Mode backup" /SC weekly /D FRI /ST 23:00
```

# Sample output

Sample output for a backup job:
```
PS C:\MyScripts> .\HyperV-switch-backup-restore.ps1 -Mode backup
Backup completed: C:\Backups\HyperV_SwitchBackup_20250331_185534.json
```

Sample output for a restore job:
```
PS C:\MyScripts> .\HyperV-switch-backup-restore.ps1 -Mode restore -FilePath C:\Backups\HyperV_SwitchBackup_20250331_185534.json
Switch 'vswitch-external' already exists. Skipping.
Creating switch: vswitch-private1 (Private)
Creating switch: vswitch-private2 (Private)
Creating switch: My internal vswitch 1 (Internal)
Creating switch: my internal vswitch 2 (Internal)
Restore completed from: C:\Backups\HyperV_SwitchBackup_20250331_185534.json
```


