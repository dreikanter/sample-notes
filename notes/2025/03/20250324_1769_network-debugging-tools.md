# Network debugging tools reference

Tools and commands for diagnosing network problems. Collected from actual debugging sessions.

## curl for API debugging

```bash
# Verbose mode (shows headers, SSL, timings)
curl -v https://api.example.com/endpoint

# Just headers
curl -I https://example.com

# With timing information
curl -w "@curl-format.txt" -s -o /dev/null https://example.com
```

`curl-format.txt` example:
```
time_namelookup:  %{time_namelookup}\n
time_connect:     %{time_connect}\n
time_appconnect:  %{time_appconnect}\n
time_starttransfer: %{time_starttransfer}\n
time_total:       %{time_total}\n
```

## DNS

```bash
dig example.com                        # DNS lookup
dig example.com +trace                 # trace full resolution path
dig @8.8.8.8 example.com               # query specific nameserver
nslookup example.com                   # simpler DNS lookup
host example.com                       # simple reverse/forward lookup
```

## TCP connectivity

```bash
telnet host port                       # test TCP connection
nc -zv host port                       # netcat port scan
nc -zvw 3 host port                    # with 3 second timeout
ss -tlnp                               # listen sockets (replaces netstat)
ss -tlnp | grep :8080                  # who's on port 8080
```

## Packet capture

```bash
tcpdump -i eth0 port 443               # capture HTTPS traffic
tcpdump -i any host 10.0.0.5           # traffic to/from host
tcpdump -w capture.pcap                # write to file for Wireshark
```

## HTTP/2 and TLS

```bash
openssl s_client -connect host:443     # inspect TLS certificate
openssl s_client -connect host:443 -servername sni.example.com
```

Reference: [https://man7.org/linux/man-pages/man8/ss.8.html](https://man7.org/linux/man-pages/man8/ss.8.html)
