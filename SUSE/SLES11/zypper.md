## zypper {#zypper}

#case1

add local ISO repository:

zypper addrepo /media  SUSE11SP3

#case 2

Hi Guys,

now this is solved and indeed the issue was because the original installation of the system was performed from the DVD delivered by VMware.

Therefore the file /etc/issue was wrong and this file is probably verified by cis-cat tool. I did a quick update with zypper command (assuming that OS is properly registered in SuSE) and voila:

zypper in -t product SUSE_SLES

now products installed are:

zypper pd

Loading repository data...

Reading installed packages...

S | Repository | Internal Name | Name | Version | Arch | Is Base

--+------------+-----------------+------------------------------------------------+------------+--------+--------

i | @System | SLES-for-VMware | SUSE Linux Enterprise Server 11 SP3 for VMware | 11.3-1.68 | x86_64 | Yes

i | @System | SUSE_SLES | SUSE Linux Enterprise Server 11 SP3 | 11.3-1.138 | x86_64 | No

and cis-cat works properly now.

I believe we will have to place a note somewhere on wiki about VMware – based installations….

Below is just a working log so you can ignore it…

ssh2cn-caehn01:/usr/local/bin # cis-cat-run.sh

10/26/15 23:45:22 : Checking for updates of zftrw_cis package

Specified repositories have been cleaned up.

Retrieving repository cis-cat metadata [done]

Building repository cis-cat cache [done]

Specified repositories have been refreshed.

Version in reposiory: 1.0-7

Version installed: 1.0-7

Update not required, the freshest version installed

% Total % Received % Xferd Average Speed Time Time Time Current

Dload Upload Total Spent Left Speed

139 139 139 139 0 0 114 0 0:00:01 0:00:01 --:--:-- 373

==== Process started at 10/26/15 23:45:31 ====

Detected OS as SUSE 11.3

Using JRE located at /usr/local/zftrw_cis/jres/SUSE

Using CISCAT located at /usr/local/zftrw_cis/cis-cat-full/CISCAT.jar

Using Benchmark /usr/local/zftrw_cis/cis-cat-full/benchmarks/CIS_SUSE_Linux_Enterprise_Server_11_Benchmark_v1.1.0-xccdf.xml

Storing Reports at /usr/local/zftrw_cis/reports

10/26/15 23:45:31 : Running CISCAT with the following options:

-a -s -x -r /usr/local/zftrw_cis/reports -b /usr/local/zftrw_cis/cis-cat-full/benchmarks/CIS_SUSE_Linux_Enterprise_Server_11_Benchmark_v1.1.0-xccdf.xml

This is CIS-CAT version 3.0.18

Loading Benchmark...

Validating Benchmark...

******************************************************

Loading Checklist Components... &lt;1 second Done

1/155 Ensure Samba is not enabled &lt;1 second N/C

2/155 Verify No Legacy &quot;+&quot; Entries Exist in /etc/passwd File &lt;1 second Pass