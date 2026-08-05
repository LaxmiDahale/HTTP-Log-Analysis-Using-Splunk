# HTTP Log Analysis Using Splunk
<img width="638" height="193" alt="image" src="https://github.com/user-attachments/assets/45723aa3-a0b7-4d61-b386-84b3c91dcc47" />

## 📌 Project Overview

This project demonstrates how to analyze HTTP traffic logs using **Splunk Enterprise** and **SPL (Search Processing Language)**.

The project uses **Zeek-style HTTP logs in JSON format** to identify HTTP errors, suspicious User-Agents, suspicious URI access attempts, abnormal web activity, and large file transfers that may indicate potential data exfiltration.

---

## 🎯 Project Objectives

* Ingest and analyze HTTP logs using Splunk Enterprise
* Detect client-side HTTP errors (**4xx**)
* Detect server-side HTTP errors (**5xx**)
* Identify suspicious or automated User-Agents
* Detect suspicious URI access attempts
* Identify large file transfers
* Monitor abnormal HTTP activity
* Gain hands-on experience with SPL
* Develop practical SIEM and SOC analysis skills

---

## 🛠️ Technologies and Tools

| Technology/Tool       | Purpose                                                |
| --------------------- | ------------------------------------------------------ |
| **Splunk Enterprise** | Log ingestion, analysis, monitoring, and visualization |
| **SPL**               | Searching and analyzing HTTP log events                |
| **Zeek HTTP Logs**    | HTTP traffic log source                                |
| **JSON**              | Log data format                                        |
| **Splunk Web**        | Data upload and security monitoring                    |
| **HTTP**              | Web traffic protocol analyzed                          |

---

## ⚙️ Lab Configuration

```text
Index: http_lab
Sourcetype: json or zeek:http
Log Format: JSON
SIEM Platform: Splunk Enterprise
```

---

## 📥 Data Ingestion Process

1. Open **Splunk Web**.
2. Navigate to:

```text
Settings → Add Data
```

3. Select **Upload**.
4. Upload the HTTP log file:

```text
http_logs.json
```

5. Configure the source type:

```text
json
```

or:

```text
zeek:http
```

6. Select the index:

```text
http_lab
```

7. Complete the upload.
8. Verify that the HTTP logs are successfully indexed.

---

# 🔍 SPL Queries and Analysis

## 1. Top Source IP Addresses Generating HTTP Traffic

```spl
index=http_lab
| stats count by "id.orig_h"
| sort -count
| head 10
```
<img width="1125" height="546" alt="image" src="https://github.com/user-attachments/assets/34910cba-85fa-40b9-9034-94615d1b56e5" />

### Purpose

Identifies the top 10 source IP addresses generating HTTP requests.

---

## 2. Detect Server Errors — HTTP 5xx

```spl
index=http_lab status_code>=500 status_code<600
| stats count as server_errors
```
<img width="1116" height="597" alt="image" src="https://github.com/user-attachments/assets/cff4af4d-6786-43b1-beea-24af89cac091" />

### Purpose

Detects server-side errors that may indicate:

* Backend application failures
* Web-server issues
* Application crashes
* Service availability problems

---

## 3. Detect Suspicious or Scripted User-Agents

```spl
index=http_lab
user_agent IN ("sqlmap/1.5.1", "curl/7.68.0", "python-requests/2.25.1", "botnet-checker/1.0")
| stats count by user_agent
```
<img width="1090" height="514" alt="image" src="https://github.com/user-attachments/assets/6c52525e-b0e1-4818-aa3f-7ef04ed44f49" />

### Purpose

Identifies automated tools that may be associated with:

* Web reconnaissance
* Automated scanning
* Scripted HTTP requests
* Suspicious web activity

---

## 4. Detect Large File Transfers

```spl
index=http_lab resp_body_len>500000
| table ts "id.orig_h" "id.resp_h" uri resp_body_len
| sort -resp_body_len
```
<img width="1090" height="547" alt="image" src="https://github.com/user-attachments/assets/8bda6568-cc24-4f98-881a-b26fc52a9795" />

### Purpose

Identifies HTTP responses larger than **500 KB**.

Large transfers may indicate:

* Unusual downloads
* Abnormal data movement
* Potential data exfiltration

---

## 5. Detect Suspicious URI Access Attempts

```spl
index=http_lab
uri IN ("/admin", "/shell.php", "/etc/passwd")
| stats count by uri, "id.orig_h"
```
<img width="1120" height="534" alt="image" src="https://github.com/user-attachments/assets/ba99e01e-1422-48c0-baa1-99b6e9ace2c3" />

### Purpose

Detects attempts to access:

* Administrative pages
* Potential web shells
* Sensitive system files

---

# 🚨 Security Use Cases

This project can help security teams detect:

* Suspicious web reconnaissance
* Automated scanning activity
* Abnormal HTTP behavior
* Suspicious User-Agent activity
* Unauthorized access attempts
* Access attempts to sensitive URIs
* Server-side application errors
* Potential data exfiltration

---

# 📊 Suggested Splunk Dashboard

The following dashboard panels can be created:

* Total HTTP Requests
* HTTP Status Code Distribution
* Top Source IP Addresses
* Top Requested URIs
* HTTP 4xx Error Trend
* HTTP 5xx Error Trend
* Suspicious User-Agent Activity
* Large File Transfer Events
* Suspicious URI Access Attempts

---

# 📈 Key Learning Outcomes

Through this project, I gained hands-on experience in:

* Splunk Enterprise
* SPL Query Development
* HTTP Log Analysis
* Zeek Log Analysis
* JSON Log Ingestion
* SIEM Monitoring
* Security Event Investigation
* Web Threat Detection
* SOC Analyst Workflows
* Blue Team Monitoring

---

# 🔮 Future Enhancements

* Create alerts for repeated HTTP 5xx errors
* Configure alerts for suspicious User-Agents
* Generate alerts for large HTTP file transfers
* Build an interactive Splunk dashboard
* Integrate threat intelligence feeds
* Integrate IDS security alerts
* Add IP reputation analysis
* Implement automated security notifications

---


# 👩‍💻 Author

**Laxmi Dahale**

Aspiring Cybersecurity Analyst | SOC Analyst | Splunk | SIEM | Network Security | System Administration

GitHub: https://github.com/LaxmiDahale

---

# 📄 Conclusion

This project demonstrates how Splunk can be used to analyze HTTP traffic, identify suspicious web activity, detect security-related anomalies, and improve visibility into web-server events.

It is a hands-on cybersecurity project designed to strengthen practical skills for **SOC Analyst, Cybersecurity Analyst, SIEM Analyst, and Blue Team roles**.
