---
layout: default
---

# Network Reconnaissance and Service Enumeration Using Nmap

## 1. Objective

The objective of this project was to perform network reconnaissance and service enumeration in an isolated virtual laboratory using Nmap.

The experiment focused on discovering active hosts, identifying open TCP ports, detecting running services and their versions, and analysing potential security concerns.

## 2. Environment and Tools

The laboratory environment consisted of two virtual machines running on VMware Workstation.

| Component | Description |
|---|---|
| Virtualisation | VMware Workstation |
| Attacker Machine | Kali Linux |
| Target Machine | Metasploitable2 |
| Network | Host-only network |
| Scanning Tool | Nmap |
| Target IP | 192.168.52.130 |

Metasploitable2 is an intentionally vulnerable virtual machine used for cybersecurity training. Both virtual machines were connected to an isolated host-only network.

## 3. Methodology

### Step 1: Laboratory Setup

Kali Linux and Metasploitable2 were configured using VMware Workstation.

Both virtual machines were connected to the same host-only network to ensure that the experiment remained isolated from external networks.

The target IP address was identified as `192.168.52.130`.

Network connectivity was tested using:

```bash
ping -c 4 192.168.52.130
```

### Step 2: Host Discovery

Nmap was used to discover active hosts on the laboratory network.

```bash
sudo nmap -sn 192.168.52.0/24
```

The `-sn` option performs host discovery without port scanning.

### Step 3: TCP SYN Port Scanning

A TCP SYN scan was performed to identify open TCP ports on the target machine.

```bash
sudo nmap -sS 192.168.52.130
```

The `-sS` option sends TCP SYN probes and analyses responses to determine port states.

### Step 4: Service Version Detection

Service version detection was performed using:

```bash
sudo nmap -sV 192.168.52.130
```

The `-sV` option attempts to identify running services and their software versions.

## 4. Results and Findings

The Nmap service version scan identified **23 open TCP ports** on the Metasploitable2 virtual machine (`192.168.52.130`).

The results revealed several network services, including FTP, SSH, Telnet, HTTP, SMB, MySQL, PostgreSQL, and VNC.

### Nmap Scan Results

The following table presents the complete service detection results.

| Port | State | Service | Version |
|---|---|---|---|
| 21/tcp | open | ftp | vsftpd 2.3.4 |
| 22/tcp | open | ssh | OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0) |
| 23/tcp | open | telnet | Linux telnetd |
| 25/tcp | open | smtp | Postfix smtpd |
| 53/tcp | open | domain | ISC BIND 9.4.2 |
| 80/tcp | open | http | Apache httpd 2.2.8 ((Ubuntu) DAV/2) |
| 111/tcp | open | rpcbind | 2 (RPC #100000) |
| 139/tcp | open | netbios-ssn | Samba smbd 3.X - 4.X (WORKGROUP) |
| 445/tcp | open | netbios-ssn | Samba smbd 3.X - 4.X (WORKGROUP) |
| 512/tcp | open | exec | netkit-rsh rexecd |
| 513/tcp | open | login? |  |
| 514/tcp | open | shell | Netkit rshd |
| 1099/tcp | open | java-rmi | GNU Classpath grmiregistry |
| 1524/tcp | open | bindshell | Metasploitable root shell |
| 2049/tcp | open | nfs | 2-4 (RPC #100003) |
| 2121/tcp | open | ftp | ProFTPD 1.3.1 |
| 3306/tcp | open | mysql | MySQL 5.0.51a-3ubuntu5 |
| 5432/tcp | open | postgresql | PostgreSQL DB 8.3.0 - 8.3.7 |
| 5900/tcp | open | vnc | VNC (protocol 3.3) |
| 6000/tcp | open | X11 | access denied |
| 6667/tcp | open | irc | UnrealIRCd |
| 8009/tcp | open | ajp13 | Apache Jserv (Protocol v1.3) |
| 8180/tcp | open | http | Apache Tomcat/Coyote JSP engine 1.1 |

### Security Analysis

The scan identified multiple exposed services that may introduce security risks.

**FTP Services (Ports 21 and 2121)**
The target runs vsftpd 2.3.4 and ProFTPD 1.3.1. Traditional FTP may transmit authentication credentials without encryption.
**Recommendation:** Use secure file transfer protocols such as SFTP and disable unnecessary FTP services.

**Telnet (Port 23)**
Telnet generally transmits login credentials and session data without encryption, making it vulnerable to network eavesdropping.
**Recommendation:** Disable Telnet and use SSH for secure remote administration.

**SMB Services (Ports 139 and 445)**
The target exposes Samba services that support network file sharing. Incorrect configurations or outdated versions may increase the risk of unauthorised access.
**Recommendation:** Restrict SMB access to trusted hosts, review shared resources, and apply security updates.

**Remote Shell Services (Ports 512, 513 and 514)**
The scan detected legacy remote access services, including rexecd and rshd. These services may lack modern authentication and encryption protections.
**Recommendation:** Disable unnecessary legacy remote access services and use SSH instead.

**Bind Shell (Port 1524)**
Nmap identified a service described as "Metasploitable root shell" on TCP port 1524. This is a particularly concerning finding because it suggests the presence of a remote shell service.
**Recommendation:** Remove unauthorised shell services, restrict remote access, and investigate unexpected listening ports.

**Database Services (Ports 3306 and 5432)**
MySQL and PostgreSQL were accessible on the target. Exposed database services may create security risks if authentication or network access controls are weak.
**Recommendation:** Restrict database access to authorised hosts, enforce strong authentication, and install security updates.

**Web Services (Ports 80 and 8180)**
The target runs Apache HTTP Server and Apache Tomcat. The detected software versions may require further security assessment.
**Recommendation:** Review software versions, apply appropriate patches, disable unnecessary features, and use HTTPS where sensitive information is transmitted.

An open port or an outdated version does not automatically confirm that a vulnerability is exploitable. Further verification is required.

## 5. Supporting Evidence

The following screenshots and scan reports document the laboratory experiment.

### Laboratory Network

![Laboratory Network](evidence/lab-network.png)

### Host Discovery

![Host Discovery](evidence/host-discovery.png)

### TCP SYN Scan

![TCP SYN Scan](evidence/port-scan.png)

### Service Version Detection

![Service Version Detection](evidence/service-scan.png)

### Scan Report

[View Nmap Scan Report](evidence/nmap-service-scan.txt)

## 6. Reflection and Improvements

This project helped me understand how Nmap can be used to perform network reconnaissance and identify exposed services.

I learned the differences between host discovery, TCP port scanning, and service version detection. The experiment also improved my ability to interpret port states and analyse network service information.

One important lesson was that identifying an open port does not necessarily mean that a system is vulnerable. Additional investigation is needed to understand the actual security risks.

In future experiments, I would improve my work by comparing different scanning techniques, checking detected software versions against security advisories, and documenting more detailed security recommendations.

## 7. Ethical Considerations

All scanning activities were conducted in an isolated laboratory using virtual machines under my control.

No unauthorised systems or external networks were targeted.

The purpose of this project was educational and focused on understanding network reconnaissance techniques.
