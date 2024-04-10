# check_aix_disk_paths
nagios check to verify AIX disk paths (vscsi, fibre channel)

This script will validate the following features for disks using MPIO as the multipathing driver:
```
    - each hdisk has paths to at least 2 vscsi or fscsi adapters
    - each hdisk has at least 2 paths
    - no paths are in one of the following states: Disabled, Failed, Unknown, Defined
    - all paths are in the "Enabled" state (which is kinda assumed by the previous line)
```

# Requirements
perl, ssh on nagios server

# Configuration

This script is executed remotely on a monitored system by the NRPE or check_by_ssh methods available in nagios.  

If you are using the check_by_ssh method, you will need to add a section similar to the following to the services.cfg file on the nagios server.  
This assumes you already have ssh key pairs configured.
```
    define service {
       use                             generic-service
       hostgroup_name                  all_aix
       service_description             Check AIX disk paths
       check_command                   check_by_ssh!/usr/local/nagios/libexec/check_aix_disk_paths
       }
```

If you are using the check_nrpe method, you will need to add a section similar to the following to the services.cfg file on the nagios server.  
This assumes you already have ssh key pairs configured.
```
    define service {
       use                             generic-service
       hostgroup_name                  all_aix
       service_description             Check AIX disk paths
       check_command                   check_nrpe!check_aix_disk_paths
       }
```

If you are using the NRPE method, you will also need a command definition similar to the following on each monitored host in the /usr/local/nagios/nrpe/nrpe.cfg file:
```
    command[check_aix_disk_paths]=/usr/local/nagios/libexec/check_aix_disk_paths
```


# Sample Output

```
AIX_DISK_PATHS OK - 200 enabled / 0 failed / 0 missing / 0 disabled / 0 defined / 0 unknown | enabled=200;;;; missing=0;;;; failed=0;;;; disabled=0;;;; defined=0;;;; unknown=0;;;;
```
