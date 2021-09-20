## Debugging Traffic in Docker
### TCP Dump
The following can be used to generate a TCP dump of docker network communication. It's assumed that the host already has docker installed and running.
```
docker build -t tcpdump - <<EOF 
FROM ubuntu 
RUN apt-get update && apt-get install -y tcpdump 
CMD tcpdump -i eth0 
EOF
```
Attach the tcpdump to the container we're interested in.
```
docker run -it --net=container:<<containerhere>> tcpdump
```
