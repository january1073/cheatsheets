# TShark Cheatsheet
![Collection](https://img.shields.io/badge/Collection-DC143C?style=flat-square&logoColor=white) ![Credential Access](https://img.shields.io/badge/Credential%20Access-DC143C?style=flat-square&logoColor=white) ![Discovery](https://img.shields.io/badge/Discovery-DC143C?style=flat-square&logoColor=white)

## Basic Commands

| Command | Description |
|---------|-------------|
| tshark -D | List available interfaces |
| tshark -i <interface> | Capture from a specific interface |
| tshark -i <interface> -w file.pcap | Capture to a file |
| tshark -r file.pcap | Read and analyze from a capture file |
| tshark -r file.pcap -c 100 | Read only first 100 packets |
| tshark -f "port 80" | Capture with a filter |
| tshark -r file.pcap -Y "http" | Apply display filter |
| tshark -G fields | List all available fields |
| tshark -V | Verbose output (with packet details) |

## Capture Options

| Command | Description |
|---------|-------------|
| tshark -i eth0 -b filesize:1000 -b files:5 | Rotate capture files (1000KB, 5 files max) |
| tshark -i eth0 -a duration:3600 | Capture for 1 hour then stop |
| tshark -i eth0 -p | Capture in non-promiscuous mode |
| tshark -i eth0 -s 96 | Snap length (capture only first 96 bytes of each packet) |
| tshark -i eth0 -n | Don't resolve names (faster) |
| tshark -i eth0 -f "sampling rate 10" | Sample packets (capture 1 of every 10 packets) |
| tshark -i eth0 -b filesize:100000 -b files:10 -w capture.pcap | Use ring buffers for continuous monitoring |
| tshark -i eth0 -f "tcp port 80 or tcp port 443 or udp port 53" | Capture only specific protocols to reduce load |

## Output Formatting

| Command | Description |
|---------|-------------|
| tshark -r file.pcap -T fields -e http.host -e http.request.uri | Extract specific fields |
| tshark -r file.pcap -T fields -E header=y -E separator=, | Output as CSV with headers |
| tshark -r file.pcap -T fields -E quote=d -E occurrence=f | Quote values and show first occurrence |
| tshark -r file.pcap -T pdml | Output as PDML (XML) |
| tshark -r file.pcap -T json | Output as JSON |
| tshark -r file.pcap -T ek | Output in Elasticsearch format |

## Field Extraction Examples

| Purpose | Command |
|---------|---------|
| HTTP hostnames & URIs | tshark -r capture.pcap -Y "http.request" -T fields -e http.host -e http.request.uri |
| Authentication usernames | tshark -r capture.pcap -Y "ntlmssp.auth" -T fields -e ntlmssp.auth.username -e ip.src |
| DNS queries | tshark -r capture.pcap -Y "dns.flags.response == 0" -T fields -e frame.time -e ip.src -e dns.qry.name |
| HTTP POST data | tshark -r capture.pcap -Y "http.request.method == POST" -T fields -e http.file_data -e http.request.uri |
| TLS server names (SNI) | tshark -r capture.pcap -Y "tls.handshake.type == 1" -T fields -e ip.src -e tls.handshake.extensions_server_name |
| HTTP traffic to CSV | tshark -r capture.pcap -Y "http" -T fields -e frame.time -e ip.src -e http.request.full_uri -E header=y -E separator=, -E quote=d -E occurrence=f > http_traffic.csv |

## Security Analysis

### Network Reconnaissance

| Purpose | Command |
|---------|---------|
| Top talkers | tshark -r capture.pcap -q -z ip_hosts,top |
| Port scanning detection | tshark -r capture.pcap -Y "tcp.flags.syn==1 and tcp.flags.ack==0" -T fields -e ip.src -e ip.dst -e tcp.dstport |
| Protocol summary | tshark -r capture.pcap -q -z io,phs |
| Network conversations | tshark -r capture.pcap -q -z conv,tcp |
| SSH brute force attempts | tshark -r capture.pcap -Y "tcp.port == 22 and tcp.flags.syn == 1 and tcp.flags.ack == 0" -T fields -e ip.src | sort | uniq -c | sort -nr |

### Authentication Analysis

| Purpose | Command |
|---------|---------|
| NTLM authentication | tshark -r capture.pcap -Y "ntlmssp" -T fields -e frame.time -e ip.src -e ip.dst -e ntlmssp.auth.username |
| Kerberos usernames | tshark -r capture.pcap -Y "kerberos.CNameString" -T fields -e frame.time -e ip.src -e kerberos.CNameString |
| HTTP basic auth | tshark -r capture.pcap -Y "http.authorization" -T fields -e frame.time -e ip.src -e http.authorization |
| Failed login attempts | tshark -r capture.pcap -Y "smb.nt_status == 0xc000006d" -T fields -e ip.src -e smb.native_lanman |

### Malware & C2 Detection

| Purpose | Command |
|---------|---------|
| Unusual DNS queries | tshark -r capture.pcap -Y "dns.qry.name and dns.qry.name matches '[a-z0-9]{25,}'" -T fields -e dns.qry.name -e ip.src |
| DNS TXT records | tshark -r capture.pcap -Y "dns.txt" -T fields -e dns.qry.name -e dns.txt |
| Beaconing detection | tshark -r capture.pcap -Y "ip.addr==192.168.1.100" -T fields -e frame.time -e ip.dst |
| Suspicious domains | tshark -r capture.pcap -Y "dns" -T fields -e dns.qry.name | sort | uniq -c | sort -rn |
| Unusual TLS certificates | tshark -r capture.pcap -Y "ssl.handshake.certificate" -T fields -e x509sat.uTF8String -e x509sat.printableString |

### SSL/TLS Analysis

| Purpose | Command |
|---------|---------|
| TLS versions used | tshark -r capture.pcap -Y "ssl" -T fields -e ssl.handshake.version | sort | uniq -c |
| Weak cipher suites | tshark -r capture.pcap -Y "ssl.handshake.ciphersuite" -T fields -e ssl.handshake.ciphersuite -e ip.dst | sort | uniq |
| Certificate details | tshark -r capture.pcap -Y "ssl.handshake.certificate" -T fields -e x509sat.uTF8String -e x509sat.printableString |
| TLS decryption | tshark -r capture.pcap -o "ssl.keys_list:192.168.1.10,443,http,server.key" -Y "http" |
| HTTP/2 headers | tshark -r capture.pcap -o "ssl.keys_list:192.168.1.10,443,http,server.key" -Y "http2" -T fields -e http2.header.name -e http2.header.value |

### Lateral Movement Detection

| Purpose | Command |
|---------|---------|
| SMB connections | tshark -r capture.pcap -Y "smb or smb2" -T fields -e frame.time -e ip.src -e ip.dst -e smb2.filename |
| WinRM connections | tshark -r capture.pcap -Y "http.request.uri contains \"wsman\"" |
| RDP connections | tshark -r capture.pcap -Y "tcp.port==3389" -T fields -e ip.src -e ip.dst |
| PowerShell remoting | tshark -r capture.pcap -Y "tcp.port==5985 or tcp.port==5986" |
| PsExec detection | tshark -r capture.pcap -Y "smb2.filename contains \"PSEXESVC\"" |

### Data Exfiltration Detection

| Purpose | Command |
|---------|---------|
| Large file transfers | tshark -r capture.pcap -q -z conv,tcp | sort -nr -k 8 |
| DNS tunneling | tshark -r capture.pcap -Y "dns.resp.type == 16" -T fields -e dns.qry.name -e dns.txt | sort -u |
| Unusual destinations | tshark -r capture.pcap -q -z endpoints,ip | sort -nr -k 3 |
| Detect object exports | tshark -r capture.pcap --export-objects "http,./exported_files" |
| Email attachments | tshark -r capture.pcap -Y "smtp.data.fragment" -T fields -e smtp.data.fragment | grep "filename=" |

## Advanced Filtering

| Purpose | Command |
|---------|---------|
| AND operation | tshark -r capture.pcap -Y "ip.src==10.0.0.1 and http" |
| OR operation | tshark -r capture.pcap -Y "tcp.port==80 or tcp.port==443" |
| Time range filter | tshark -r capture.pcap -Y "frame.time >= \"Jan 01, 2023 00:00:00\" and frame.time <= \"Jan 01, 2023 01:00:00\"" |
| Substring match | tshark -r capture.pcap -Y "http.user_agent contains \"Mozilla\"" |
| Regular expression | tshark -r capture.pcap -Y "http.host matches \"^[a-z0-9]{15,}\\.com$\"" |
| Packet size filter | tshark -r capture.pcap -Y "frame.len > 1000" |

Reach out: https://linktr.ee/january1073
