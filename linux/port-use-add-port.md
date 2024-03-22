# Ver puertos si estan abiertos
```
sudo lsof -i -P -n | grep LISTEN | grep 8001
sudo netstat -tulpn | grep LISTEN | grep 8001
sudo ss -tulpn | grep LISTEN | grep 8001
sudo lsof -i -P -n | grep 8001
sudo lsof -i:8001

cat /etc/services | grep 8001

netstat -tulpn | grep LISTEN
netstat -tulpn | more
netstat -tulpn | grep ':8001' 

Where netstat command options are:

    -t : Select all TCP ports
    -u : Select all UDP ports
    -l : Show listening server sockets (open TCP and UDP ports in listing state)
    -p : Display PID/Program name for sockets. In other words, this option tells who opened the TCP or UDP port. For example, on my system, Nginx opened TCP port 80/443, so I will /usr/sbin/nginx or its PID.
    -n : Don’t resolve name (avoid dns lookup, this speed up the netstat on busy Linux/Unix servers)

```

## Agregar un puerto 
- editas el archivo.... /etc/sysconfig/iptables y le copias una de las lineas que tiene un puerto ya configurado y solo le cambias el puerto
```
# Agregar a iptables
 iptables -I INPUT -p tcp --dport 5000 -j ACCEPT
# permanente
    -A INPUT -p tcp -m state --state NEW -m tcp --dport 8000 -j ACCEPT
```

## Ver servicios corriendo en un puerto
```
[root@My Server]#  netstat -nlp | grep :9092
tcp6      51      0 :::9092                 :::*                    LISTEN      29849/java
[root@My Server]#  netstat -tulnp | grep :9092
tcp6      51      0 :::9092                 :::*                    LISTEN      29849/java
[root@My Server]# netstat -tulpn | grep :9092
tcp6      51      0 :::9092                 :::*                    LISTEN      29849/java
[root@My Server]# ps -aufx | grep 29849
root      4292  0.0  0.0 112640   968 pts/0    S+   15:42   0:00                          \_ grep --color=auto 29849
root     29849  0.4  1.6 5997988 725468 ?      Sl   Apr11 1018:51 /bin/java -Xmx256M -Djetty.logging.dir=/opt/WSAPICommons/jetty-9.3.6/logs -Djetty.home=/opt/WSAPICommons/jetty-9.3.6 -Djetty.base=/opt/WSAPICommons/jetty-9.3.6 -Djava.io.tmpdir=/tmp -jar /opt/WSAPICommons/jetty-9.3.6/start.jar jetty.state=/opt/WSAPICommons/jetty-9.3.6/jetty.state jetty-logging.xml jetty-started.xml
```