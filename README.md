# ddos-deflate-custom

Fork of DDoS Deflate http://deflate.medialayer.com/ with fixes, improvements and new features.

Original Author: Zaf zaf@vsnl.com (Copyright (C) 2005)

**Maintainer:** Jefferson González <jgmdev@gmail.com> - https://github.com/jgmdev/ddos-deflate

**Maintainer 2:** msergiy87 <sergiy_007@ukr.net>

The main goal of this script - defense Hosting server.

The common problem is that ddos-deflate script ban address of search systems. We try to solve it.

Added Feaches
------------

- prevent block some predefined local and trusted network address
- prevent block some predefined search systems address
- prevent block some connections to FTP (server address) with some ports
- create separate iptables chain for ddos-deflate
- prevent block some address from files joomla_admins.conf and search_system_ip.conf in which address add automatically by another scripts

Distros tested
------------

Currently, this is only tested on Debian 7.9. It should theoretically work on older versions of Ubuntu or Debian based systems.

Installation
------------

##### 1) install dsniff

```shell
apt-get install dsniff
```

##### 2) Install ddos-deflate from this repository https://github.com/jgmdev/ddos-deflate.
As root user execute the following commands:

```shell
cd /usr/src
wget https://github.com/jgmdev/ddos-deflate/archive/master.zip
unzip master.zip
cd ddos-deflate-master
./install.sh
```

##### 3) Just move files from root_scripts to a folder /root/scripts

```shell
cd /tmp
wget https://github.com/msergiy87/ddos-deflate-custom/archive/master.zip
unzip master.zip
cd ddos-deflate-custom-master
mkdir /root/scripts
mv root_scripts/* /root/scripts/
```

##### 4) And replace your ddos.sh file

```shell
cp /usr/local/ddos/ddos.sh /usr/local/ddos/ddos.sh_backup
cp ddos.sh /usr/local/ddos/ddos.sh
```

##### 5) And configure ddos.conf file

```
EMAIL_TO="hosting-security@example.com"
BAN_PERIOD=1800
```

##### 6) And this check (for creation iptables chain) to cron file

```
*/31 * * * *  /usr/local/ddos/ddos.sh -n > /dev/null 2>&1
```

Uninstallation
------------

As root user execute the following commands:

```shell
cd ddos-deflate-master
./uninstall.sh
```

Usage
------------
Data in the files exclude_variables.conf or joomla_admins.conf or search_system_ip.conf is like example. You should change it.

Major changes, file ddos.sh
------------

##### 1) Download variables from the file exclude_variables.conf (single point of reading for multiple applications) to exclude from the analysis and blocking:
- certain internal network address (TRUST_NET, considered safe).
- some problematic users address (SOME_PROBLEM_USERS).
- networks search engines (Search systems) - GOOGLE YANDEX MAILRU META YAHOO
- server address and FTP ports

EXCLUDE - defines the list of all address and templates that should be excluded from the analysis.
```
EXCLUDE="$TRUST_NET|$GOOGLE|$YANDEX|$MAILRU|$META|$YAHOO|$SOME_PROBLEM_USERS"
```
##### 2) Create iptables chain for ddos-deflate and forward to it all input traffic.

##### 3) Add to ignore_list my custom trusted ipaddress from files:
- exclude analysis address from files joomla admin.conf (JOOMLA ADM) and search_system_ip.conf (SEARCH_SYS_IP). Other scripts write address of Joomla admins and address search engines that found other scripts (whois command and its analize) and recorded in the files.

##### 4) Change command netstat:
- exclude analysis of specific ports FTP, which work is set Pure-FTPd (PUREFTP) 70000-72999
- exclude all contained in EXCLUDE
