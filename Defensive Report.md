# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- Hypervisor / Host Machine 
  - **Operating System**: Microsoft Windows
  - **Purpose**: Gateway
  - **IP Address**: 192.168.1.1
- ELK
  - **Operating System**: Linux
  - **Purpose**: Elasticsearch, Logstash, Kibana Server
  - **IP Address**: 192.168.1.100
- Capstone
  - **Operating System**: Linux
  - **Purpose**: Basic HTTP Server (this is a red herring)
  - **IP Address**: 192.168.1.105
- Target 1
  - **Operating System**: Linux
  - **Purpose**: HTTP Server (also wordpress site)
  - **IP Address**: 192.168.1.110
- Target 2
  - **Operating System**: Linux
  - **Purpose**: HTTP Server
  - **IP Address**: 192.168.1.115

### Description of Targets

Two Apache web servers on the network had SSH enabled, causing a potential attack through port 80 and 22. 
- Target Machine 1 with the IP Address of 192.168.1.110
- Target Machine 2 with the IP Address of 192.168.1.115

### Monitoring the Targets

Performing an nmap scan, we can determine potential points of entry though the 'ssh' and 'http' services:
(Insert screenshot of evidence)

Traffic to these services should be carefully monitored. So it is crucial to implemented the alerts to do so:

#### Excessive HTTP Erros

Metric
- `WHEN count() GROUPED OVER top 5 'http.response.status_code'`

Threshold
- `IS ABOVE 400`
 
Vulnerability Mitigated
- `Enumeration/Brute Force`

Reliability
 -This alert is highly reliable because it measures by error codes rated 400 and above, allowing visibility of potential threats on the server.
 
 (Insert status of this alert)

#### HTTP Request Size Monitor
Metric
- `WHEN sum() of http.request.bytes OVER all documents`

Threshold
- `IS ABOVE 3500`
 
Vulnerability Mitigated
- `Code injection in HTTP requests (XSS and CRLF) or DDOS`

Reliability
 -This would be medium reliability because alerts can be false positive due to non-malicious or legitimage HTTP requests and traffic within the server.
 
 (Insert status of this alert)
 
 
#### CPU Usage Monitor
Metric
- `WHEN max() OF system.process.cpu.total.pct OVER all documents`

Threshold
- `IS ABOVE 0.5`
 
Vulnerability Mitigated
- `Malicious software and programs taking up resources`

Reliability
 -This alert is highly reliable due to the malicious programs running that can determine CPU Usage.
 
 (Insert status of this alert)



### Suggestions for Going Further (Optional)
_TODO_: 
- Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain _how_ to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
- Vulnerability 1
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 2
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 3
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
