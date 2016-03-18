# ddos-deflate-custom

Fork of DDoS Deflate http://deflate.medialayer.com/ with fixes, improvements and new features.

Original Author: Zaf zaf@vsnl.com (Copyright (C) 2005)

**Maintainer:** Jefferson González <jgmdev@gmail.com> - https://github.com/jgmdev/ddos-deflate

**Maintainer 2:** msergiy87 <sergiy_007@ukr.net>

Debian Wheezy suport

The main goal of this script - defense Hosting server.

The common problem is that ddos-deflate script ban address of search systems. We try to solve it.

##### Added Feaches:
- prevent block some predefined local network address
- prevent block some predefined search systems address
- prevent block some connections to FTP (server address) with some ports
- create separate iptables chain for ddos-deflate
- prevent block some address from files joomla_admins.conf and search_system_ip.conf in which address add automatically by another scripts

##### Recomendations:

- install dsniff - apt-get install dsniff
- Install ddos-deflate from this repository https://github.com/jgmdev/ddos-deflate
- Just move files from root_scripts to a folder /root/scripts
- And replace your ddos.sh file.
- And configure ddos.conf file

##### Major changes, file ddos.sh:

###### 1) Download variables from the file exclude_variables.conf (single point of reading for multiple applications) to exclude from the analysis and blocking:
- certain internal network address (LOCAL_NET, considered safe).
- some problematic users address (SOME_PROBLEM_USERS).
- networks search engines (Search systems) - GOOGLE YANDEX MAILRU META YAHOO
- server address and FTP ports

EXCLUDE - defines the list of all address and templates that should be excluded from the analysis.
```
EXCLUDE="$LOCAL_NET|$GOOGLE|$YANDEX|$MAILRU|$META|$YAHOO|$SOME_PROBLEM_USERS"
```
###### 2) Create iptables chain for ddos-deflate and forward to it all input traffic.

###### 3) Add to ignore list my custom trusted ips from files:
- exclude analysis address from files joomla admin.conf (JOOMLA ADM) and search_system_ip.conf (SEARCH_SYS_IP). Other scripts write address of Joomla admins and address search engines that found other scripts (whois command and its analize) and recorded in the file.

###### 4) Change command netstat:
- exclude analysis of specific ports FTP, which work is set Pure-FTPd (PUREFTP) 70000-72999
- exclude all contained in EXCLUDE
