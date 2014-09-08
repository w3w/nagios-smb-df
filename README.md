nagios-smb-df
===============

Nagios plugin to check available free space of smb-connected partition. Classic nagios `check_disk` reports free space on the remote partition, rather than amount of disk quota left. This plugin uses `smbclient` to get the correct values.

Usage:
--------

`check_smb_df warning_percent_level critical_percent_level smb_user
 smb_password smb_url`

Add to your nagios config:
--------------------------------

Define the command first (assuming you have `check_smb_df` in `/opt`)
````
define command{
    command_name    check_smb_df
    command_line    /opt/check_smb_df $ARG1$ $ARG2$ $ARG3$ $ARG4$ $ARG5$
}

````

Then define the service for a host with 10% warning and 5% critical:
````
define service{
    use                 local-service
    host_name           localhost
    service_description SMB Disk Check
    check_command       check_smb_df!10!5!username!password!//smb.server/url
}
````

Service check sends also a message `OK: 66 GB free of 100 GB (66 %)`
