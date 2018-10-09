last updated : 2018-10-09<br>
<br>
Set of scripts used to monitor how much disk space is available on AWS EC2 instances.


## `disk-report`
<br>
Creates report for disk space availability on different mounts/Filesystems within an AWS EC2, Ubutnu instance.  Reports are printed to stdout, and will use AWS SNS functionality to issue a warning if disk space is under a certain threshold.  Thresholds are hard coded to : 2 GB for root '/', and 10% for other mounts. Note that units in code are in MB to keep precision. <br>
<br>

Requirements :<br>
- aws-cli.  To install run : `apt get-intall` awscli on Ubuntu. <br> 
- Configure AWS by providing access codes, and access code secrets, default region, and output type.  To do this, run `aws configure` once aws-cli is installed. <br>
- SNS requires a topic ARN provided as a command line argument when running script.<br>
<br>

Usage : `./disk_usage arn:aws:sns:region_name:x-xxxxxxxxxxxx:Topic-Name`<br>
<br>
Future : <br>
- Add optional arguments to change thresholds <br>
- Run script without ARN and just get report (no messaging functionality) <br>
<br>

Note : this script was designed to be run with `disk-monitor` so that it can be done on a loop, and have periodic checks.<br>
<br>

## `disk-monitor`
Runs `disk-report` on a loop, and records latest report into a text file `disk_check.txt`.  Waiting time between checks is 12 hours.  Meant to be run using `nohup` so that it can be detached from terminal and be a constant background process. 
<br> <br>
Usage (by detaching from terminal) : `nohup ./disk-monitor arn:aws:sns:region-name:x-xxxxxxx:Topic-Name&` <br>
<br>
Future :
- Add command line option for monitioring period.

## `monitor-group`
Similar to `disk-monitor` but also loops over different EC2 instances.  Said instances are stored in a file `instances` with 2 columns : user@instancePUblicDNS | Topic ARN for sending messages.  Said instances need to have given permission for access by adding a public ssh key to ~/.ssh/authorized_keys in monitoring instance.  They also need to have been configured as descripted in in `disk-report`. <br>
<br>

Usage : <br>
`nohup ./monitor-group &` 
<br>
(does not take input since it reads from the file) <br>
<br>
Future : <br>
- Add command line options for paths of input/output files, and period (sleep time).
