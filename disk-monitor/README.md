## `disk-report`
last updated : 2018-10-09<br>
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

Note : this script was designed to be run with `disk-monitor` so that it can be done on a loop, and have periodic checks. 