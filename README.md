
# Snort Challenge: Detecting and Blocking Outbound Reverse Shells

This project demonstrates how I used **Snort** to detect and stop a reverse shell attack. The focus was on identifying outbound traffic that might indicate an attacker has already compromised the system and is communicating externally, specifically using TCP port **4444** (commonly associated with **Metasploit**).

---

## Objective

- Detect persistent outbound traffic on port 4444.
- Create an **IPS rule** to block the reverse shell activity.
- Capture the flag as evidence of successfully stopping the attack.

---

## Skills Learned

- Analyzing network traffic with Snort.
- Writing IPS rules to block malicious outbound connections.
- Identifying reverse shells in network traffic.
- Using command-line tools to investigate and filter logs.

---

## Tools Used

- **Snort**: Network Intrusion Detection and Prevention System.
- **grep**: Used to filter specific patterns in logs.
- **Metasploit**: Understanding its role in reverse shells and TCP/4444 exploitation.

---

## Steps

### 1. **Starting Snort in Sniffer Mode**
I initiated Snort in **sniffer mode** to get a real-time view of the network traffic:

```bash
sudo snort -c /etc/snort/snort.conf -q -A console
```

### 2. **Identifying Suspicious Traffic**
Using `grep`, I filtered through the Snort logs to check for outbound traffic on port **4444**:

```bash
grep -r -i -n "4444" /home/ubuntu/Desktop/Snort/
```

The persistent traffic indicated the presence of a **reverse shell**.

### 3. **Creating the IPS Rule**
I created an IPS rule to block outbound traffic to the attacker's IP (10.10.196.55) on port **4444**:

```bash
drop tcp any any -> 10.10.196.55 4444 (msg: "Logging"; sid:100002; rev:1)
```

This rule ensures that any TCP traffic to this IP on port 4444 is dropped, effectively blocking the reverse shell.

### 4. **Testing the Rule in IPS Mode**
I ran Snort in **inline mode** to apply the IPS rule and block the malicious traffic:

```bash
snort -c /etc/snort/snort.conf -Q --daq afpacket -i eth0:eth1 -A full -l /var/log/snort/
```

### 5. **Success!**
After running Snort with the new rule, I successfully blocked the outbound attack, and a flag appeared on my desktop:

THM{0ead8c494861079b1b74ec2380d2cd24}

---

## Key Takeaway

The project emphasized the importance of monitoring outbound traffic, which is often overlooked. Catching reverse shells and other outbound threats is critical in detecting attackers who may have already breached the system.

---

## References

- **Snort**: [Snort Documentation](https://www.snort.org/documents)
- **Metasploit**: [Metasploit Documentation](https://www.metasploit.com/)

