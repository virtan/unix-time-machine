Unix (rsync based) Time Machine
=================

Rsync-based Time Machine backup utility

## Why
I was using Mac OS X Time Machine to backup my laptop hard drive over Wi-Fi
to home Linux server used as NAS. It wasn't very stable requiring me to
repair occasionally broken Time Machine image every 2-3 months. Moreover, I
had to recreate sparse backup image every 4-6 months due to unrecoverable
breakdowns.

## Solution
So, I've decided to switch to unix rsync-based solution. It is pretty simple
and runs like clockwork. No one breakdown for 6 months.
I've forgotten about backing up problems. It just works in background.

## Details
Scripts creates one full copy of laptop hard drive and then creates
a delta copy (hardlinking unchanged files) every N (configurable) days.
It keeps just M (configurable) copies in total replacing oldest copy by newest one.
Being called from crontab it checks that it is in correct environment and
that time elapsed since the last copy is long enough and starts new copy.
If copy process is unsuccessful it restarts copying in next check,
removing unsuccessfully copied data.

## Consumption
Initial copy is copied as is, so requires the same amount of disk space.
Next copy depends on the amount of changed files and hardlink/dirs overhead.
In my case (256Gb disk, 200Gb used) incremental copy takes 6-30Gb of disk
(depending on the amount of changes for N days) and finishes in minutes.

## Installation
1. Ensure that root (configurable) user can ssh to server by key.
2. Change settings in utm script.
3. ```# install -o root -g root -m 700 utm /usr/bin/utm```
```# ( crontab -u root -l ; echo "20 * * * * /usr/bin/utm >>/var/log/utm-run.log 2>&1" ) \```
```  | crontab -u root -```
