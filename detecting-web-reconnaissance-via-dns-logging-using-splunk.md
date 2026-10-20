# Detecting Web Application Reconnaissance via DNS Logging using Splunk
<img width="930" height="572" alt="image" src="https://github.com/user-attachments/assets/a9f874d7-ad9a-4241-9993-ef97e196714a" />

## Introduction

DNS (Domain Name System) logs play a vital role in tracking network activity and uncovering potential security threats. Splunk SIEM (Security Information and Event Management) offers robust tools to ingest, search, and analyze DNS logs to spot anomalies or malicious domain requests across a network.

## Project Overview

In this lab experiment, we will ingest sample DNS logs into Splunk and run practical search queries to track domain queries, spot subdomain scanning, and uncover suspicious network traffic related to web applications.

## Prerequisites
<img width="520" height="320" alt="image" src="https://github.com/user-attachments/assets/25c13433-22b3-4b57-aa1c-04c263d23a21" />

Before analyzing DNS logs in Splunk, ensure the following:

- A Splunk Enterprise instance is installed and running.
- Sample DNS log files are prepared and ready to be uploaded into Splunk.
- Video tutorial to download Splunk on Windows: https://www.youtube.com/watch?v=_2O-qxS8nqI
## Steps to Upload Sample DNS Log Files to Splunk SIEM

### 1. Prepare Sample DNS Log Files

- Obtain a sample DNS log file in a readable format (such as `.txt` or `.log`).
- Ensure the log file contains relevant DNS event fields, including source IP, destination IP, domain name (FQDN), query type, and response code.
- Save the sample log file in a directory accessible from your machine.

### 2. Upload Log Files to Splunk

- Log in to the Splunk web interface.
- Navigate to **Settings** > **Add Data**.
- Select **Upload** as the data input method.

### 3. Choose File

- Click on **Select File** and choose the sample DNS log file you prepared earlier.

### 4. Set Source Type

- In the **Set Source Type** section, specify the source type for the uploaded log file.
- Set a clear source type name for your DNS logs, such as `dns_sample`.

### 5. Review Settings

- Review other input settings such as the target index, host name, and sourcetype.
- Ensure the settings are configured properly to match your sample DNS log file.

### 6. Click Upload

- Once all settings are configured, click on the **Review** button.
- Check the configuration summary one final time for accuracy.
- Click **Submit** to upload and index the sample DNS log file into Splunk.

### 7. Verify Upload

- After uploading, navigate to the search bar in the Splunk interface and set the time range to **All time**.
- Run the following search query to verify that the uploaded DNS events are visible:

```text
index=<your_dns_index> sourcetype=dns_sample
```

## Steps to Analyze DNS Log Files in Splunk SIEM

### 1. Search for DNS Events

- Open the Splunk interface and navigate to the search bar.
- Enter the following search query to retrieve all uploaded DNS events:

```text
index=* sourcetype=dns_sample
```

### 2. Extract Relevant Fields

- Identify key fields in the DNS logs such as source IP (`src_ip`), destination IP (`dest_ip`), domain name (`fqdn`), query type, and response code.
- Using `| regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"` searches for common DNS-related keywords in the raw event data.
- Example extraction and filtering command:

```text
index=* sourcetype=dns_sample | regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"
```

### 3. Identify Web Reconnaissance & Anomalies

- Look for unusual patterns or high-volume spikes in DNS lookup activity across the network.
- Example query to count domain occurrences and identify spikes:

```text
index=_* OR index=* sourcetype=dns_sample | stats count by fqdn | sort - count
```

### 4. Find the Top DNS Sources

- Use the `top` command to identify the most frequent domain requests alongside the source IP addresses generating them:

```text
index=* sourcetype=dns_sample | top fqdn, src_ip
```

### 5. Monitor DNS Query Volume Over Time

- Plot DNS lookup requests in one-hour intervals to spot sudden bursts of automated network activity:

```text
index=* sourcetype=dns_sample | timechart span=1h count
```

### 6. Investigate Suspicious Domains

- Search for domains associated with known malicious activity or unusual lookup behavior.
- Cross-reference unfamiliar domains against threat intelligence or reputation databases such as [VirusTotal](https://www.virustotal.com).
- Example search for investigating a specific suspicious domain:

```text
index=* sourcetype=dns_sample fqdn="maliciousdomain.com"
```

## Conclusion

In modern cybersecurity operations, visibility is everything. Because the Domain Name System acts as the backbone for virtually all network traffic, it is also one of the primary avenues exploited by attackers for Command-and-Control communications, data exfiltration, and malware delivery. 

By building this Splunk SIEM pipeline, you have bridged the gap between raw data collection and actionable threat intelligence. You now possess the hands-on capability to:
- Ingest and structure unstructured network logs.
- Craft precise Search Processing Language (SPL) queries to filter network noise.
- Uncover anomalous traffic spikes, failed lookups, and suspicious domain requests.

Whether you are building out a university security portfolio, setting up a student lab experiment, or stepping into your first Security Operations Center (SOC) role, practicing these foundational hunting techniques will significantly sharpen your analytical edge. 

Keep exploring, keep testing, and happy hunting! 🔍🛡️
