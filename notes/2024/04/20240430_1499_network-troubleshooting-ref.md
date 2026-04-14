# Network troubleshooting reference

Tools and sequence for diagnosing network issues. I always start too complex.

## The simple-first sequence

1. Can I ping the host? `ping -c 4 hostname`
2. Can I resolve the hostname? `dig +short hostname`
3. Can I reach the port? `nc -zv hostname port` or `telnet hostname port`
4. Is my routing correct? `traceroute hostname`
5. Is something blocking it? Check firewall rules, security groups

## Core tools

```bash
# Connectivity
ping -c 4 8.8.8.8          # test connectivity (ICMP)
ping -c 4 hostname          # test DNS + connectivity

# DNS
dig example.com             # full DNS query
dig +short example.com      # just the answer
nslookup example.com        # simpler alternative

# Port reachability
nc -zv host 443             # test if port is open (-z: scan, -v: verbose)
nc -zvw3 host 443           # with 3s timeout
curl -v telnet://host:443   # alternative

# Routing
traceroute host             # hop-by-hop path (ICMP)
traceroute -T host          # TCP traceroute (bypasses some firewalls)
mtr host                    # continuous traceroute with stats

# Connections
ss -tunlp                   # all TCP/UDP listening sockets with PIDs
ss -tnp state established   # established connections
netstat -tulnp              # same (older tool)
lsof -i :8080               # what's using port 8080
```

## HTTP debugging

```bash
curl -v https://example.com          # verbose with headers
curl -I https://example.com          # headers only
curl -w "\n%{http_code}\n" https://  # just status code
curl --resolve host:443:1.2.3.4 https://host/  # override DNS
curl --max-time 5 https://example.com  # timeout
```

## TLS inspection

```bash
openssl s_client -connect hostname:443
echo | openssl s_client -connect hostname:443 2>/dev/null | openssl x509 -noout -dates
```

Reference: https://wizardzines.com/zines/networking/
