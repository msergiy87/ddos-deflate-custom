# ddos-deflate-custom

Fork of DDoS Deflate http://deflate.medialayer.com/ with fixes, improvements and new features.

Original Author: Zaf zaf@vsnl.com (Copyright (C) 2005)

The main goal of this script - defense Hosting server.

The common problem is that ddos-deflate script ban address of search systems. We try to solve it.

Recomendations:

Just move the files from root_scripts to a folder /root/scripts

And replace your ddos.sh file.

And configure ddos.conf file

Major changes, file ddos.sh:

1) Downloads variables from the file exclude_variables.conf to exclude from the analysis and blocking:
- certain internal network addresses (LOCAL_NET, considered safe).
- addresses some problematic users (SOME_PROBLEM_USERS).
- networks search engines (Search systems) - GOOGLE YANDEX MAILRU META YAHOO.

EXCLUDE - defines the list of all addresses and templates that should be excluded from the analysis.
EXCLUDE="$LOCAL_NET|$GOOGLE|$YANDEX|$MAILRU|$META|$YAHOO|$SOME_PROBLEM_USERS"

2) Creates a Chain ddos-deflate in iptables and forwards to it all input traffic.

3) Change command netstat:
- exclude analysis of specific ports FTP, which work is set Pure-FTPd (PUREFTP) 50000-52999
- excludes all contained in EXCLUDE

4) Create file $BAD_IP_LIST_ANALIZE:
- excludes analysis addresses from files joomla admin.conf (JOOMLA ADM) and search_system_ip.conf (SEARCH_SYS_IP). These other scripts write address of Joomla admins and address search engines that found other scripts (whois command and its analize) and recorded in the file.
 
5) You can collect statistics for analysis, but do not forget to configure logrotate.

echo $CURR_LINE_CONN $CURR_LINE_IP >> /tmp/for_ddos_analiz
