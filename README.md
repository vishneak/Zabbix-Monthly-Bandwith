# Zabbix-Monthly-Bandwidth
In this repository you will find information that will help you to set up Zabbix Server to collect information regarding Monthly Bandwidth from your Zabbix Host whith vnstat (it was tested on Ubuntu 12, 14 and 16).

# Zabbix Host modifications
1) On the Zabbix Host install vnstat and bc packages
#apt-get -y install vnstal bc

2) Check if the vnstat start working and is collecting data:
:#vnstat -u -i {interface name}
:#vnstat -m

Note:
Sometimes vnstat needs more time to collect data (on some hosts it has passed ~6 hours before vnstat has started working). On the most of the hosts, vnstat has started to collect data immediately.
If you have any issues with that, please check /etc/vnstat.conf file

3) Add to your zabbix configuration folder the monthly traffic userparameter (In my case it is /etc/zabbix/zabbix_agentd.conf.d/ folder)
:#vi /etc/zabbix/zabbix_agentd.conf.d/userparameter_monthly.traffic.conf

Note:
Userparameter is called system.monthlybandwidth, you'll need that later on the Zabbix Server Web Interface for Key value and adding a new Item

4) Add to /usr/local/bin the script (zabbix_total_month_bandwidth.sh) that will provide Statistics for zabbix and make this file executable
:#vi /usr/local/bin/zabbix_total_month_bandwidth.sh
:#chmod +x /usr/local/bin/zabbix_total_month_bandwidth.sh

5) Restart Zabbix agent
:#service zabbix-agent restart

# Zabbix Server modifications
6) Add a new Item and Graph to your Zabbix Server (I have added it to Template OS Linux )
From your Zabbix Server Web Interface: Configuration->Templates->Template OS Linux->Items->Create item
https://ibb.co/fgFYCw
Configuration->Templates->Template OS Linux->Graphs->Create graph
https://ibb.co/hEG6Xw

7) After these settings were saved, go to Monitoring->Graphs and select Montlhy bandwidth graph for the host that has this Item saved
https://ibb.co/jGGUkG

# Final note:
a) vnstat usually resets all traffic on 1st day of each month.
b) You may have issues on collecting data with vnstat. I have encountered such issues with interfaces called p1p1, em1, enp0s25, etc. 
________________________________________________________________________________________________________________________

You can change the interface name and the month start date on which vnstat collects/resets data in /etc/vnstat.conf file
