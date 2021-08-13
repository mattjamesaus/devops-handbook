## Diagnosing Port Traffic
As per https://www.cyberciti.biz/faq/how-to-check-open-ports-in-linux-using-the-cli
### netstat
`sudo netstat -tulpn | grep LISTEN`
```
-t : All TCP ports
-u : All UDP ports
-l : Display listening server sockets
-p : Show the PID and name of the program to which each socket belongs
-n : Don’t resolve names
| grep LISTEN : Only display open ports by applying grep command filter.
```
### ss
`sudo ss -tulpn`
### lsof
`sudo lsof -i -P -n | grep LISTEN`
```
-i : Look for listing ports
-P : Inhibits the conversion of port numbers to port names for network files. Inhibiting the conversion may make lsof run a little faster. It is also useful when port name lookup is not working properly.
-n : Do not use DNS name
| grep LISTEN : Again only show ports in LISTEN state using the grep command as filter.
```
### nmap 
```
$ sudo nmap -sT -O localhost
$ sudo nmap -sU -O 192.168.2.254 ##[ list open UDP ports ]##
$ sudo nmap -sT -O 127.0.0.1 ##[ list open TCP ports ]##
$ sudo nmap -sTU -O 192.168.2.24
```
