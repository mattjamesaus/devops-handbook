# Network Monitoring
References: https://www.binarytides.com/linux-commands-monitor-network/

1) Overall bandwidth - nload, bmon, slurm, bwm-ng, cbm, speedometer, netload
1) Overall bandwidth (batch style output) - vnstat, ifstat, dstat, collectl
1) Bandwidth per socket connection - iftop, iptraf, tcptrack, pktstat, netwatch, trafshow
1) Bandwidth per process - nethogs

### 1. nload
Nload is a commandline tool that allows users to monitor the incoming and outgoing traffic separately.
It also draws out a graph to indicate the same, the scale of which can be adjusted. Easy and simple to use, and does not support many options.
So if you just need to take a quick look at the total bandwidth usage without details of individual processes, then nload will be handy.  
`$ nload`

### 2. iftop
Iftop measures the data flowing through individual socket connections, and it works in a manner that is different from Nload.
Iftop uses the pcap library to capture the packets moving in and out of the network adapter, and then sums up the size and count to find the total bandwidth under use.  
`$ sudo iftop -n`  
The n option prevents iftop from resolving ip addresses to hostname, which causes additional network traffic of its own.

### 3. iptraf
Iptraf is an interactive and colorful IP Lan monitor. It shows individual connections and the amount of data flowing between the hosts.   
`$ sudo iptraf`

### 4. nethogs
Nethogs is a small 'net top' tool that shows the bandwidth used by individual processes and sorts the list putting the most intensive processes on top.  
In the event of a sudden bandwidth spike, quickly open nethogs and find the process responsible. Nethogs reports the PID, user and the path of the program.  
`$ sudo nethogs`

### 5. bmon
Bmon (Bandwidth Monitor) is a tool similar to nload that shows the traffic load over all the network interfaces on the system. The output also consists of a graph and a section with packet level details.  
`$ sudo bmon`

### 6. slurm
Slurm is 'yet' another network load monitor that shows device statistics along with an ascii graph. It supports 3 different styles of graphs each of which can be activated using the c, s and l keys. Simple in features, slurm does not display any further details about the network load.  
`$ slurm -s -i eth0`

### 7. tcptrack
Tcptrack is similar to iftop, and uses the pcap library to capture packets and calculate various statistics like the bandwidth used in each connection.  
It also supports the standard pcap filters that can be used to monitor specific connections.  

### 8. Vnstat
Vnstat is bit different from most of the other tools. It actually runs a background service/daemon and keeps recording the size of data transfer all the time.
Next it can be used to generate a report of the history of network usage.  
To monitor the bandwidth usage in realtime, use the '-l' option (live mode). It would then show the total bandwidth used by incoming and outgoing data, but in a very precise manner without any internal details about host connections or processes.  
```
$ service vnstat status
```
### 9. bwm-ng
Bwm-ng (Bandwidth Monitor Next Generation) is another very simple real time network load monitor that reports a summary of the speed at which data is being transferred in and out of all available network interfaces on the system.  
`$ bwm-ng`

### 10. cbm - Color Bandwidth Meter
A tiny little simple bandwidth monitor that displays the traffic volume through network interfaces. No further options, just the traffic stats are display and updated in realtime.  
`$ sudo apt-get install cbm`

### 11. speedometer
Another small and simple tool that just draws out good looking graphs of incoming and outgoing traffic through a given interface.  
`$ speedometer -r eth0 -t eth0`

### 12. Pktstat
Pktstat displays all the active connections in real time, and the speed at which data is being transferred through them.  
It also displays the type of the connection, i.e. tcp or udp and also details about http requests if involved.  
`$ sudo pktstat -i eth0 -nt`

### 13. Netwatch
Netwatch is part of the netdiag collection of tools, and it too displays the connections between local host and other remote hosts, and the speed at which data is transferring on each connection.  
`$ sudo netwatch -e eth0 -nt`

### 14. Trafshow
Like netwatch and pktstat, trafshow reports the current active connections, their protocol and the data transfer speed on each connection. It can filter out connections using pcap type filters.  
Monitor only tcp connections  
`$ sudo trafshow -i eth0 tcp`

### 15. Netload
The netload command just displays a small report on the current traffic load, and the total number of bytes transferred since the program start. No more features are there. Its part of the netdiag.  
`$ netload eth0`

### 16. ifstat
The ifstat reports the network bandwidth in a batch style mode. The output is in a format that is easy to log and parse using other programs or utilities.  
`$ ifstat -t -i eth0 0.5`

### 17. dstat
Dstat is a versatile tool (written in python) that can monitor different system statistics and report them in a batch style mode or log the data to a csv or similar file. This example shows how to use dstat to report network bandwidth  
`$ dstat -nt`

### 18. collectl
Collectl reports system statistics in a style that is similar to dstat, and like dstat it is gathers statistics about various different system resources like cpu, memory, network etc.  
```
$ collectl -sn -oT -i0.5
```
