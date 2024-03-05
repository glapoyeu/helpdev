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

