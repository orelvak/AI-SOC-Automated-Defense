# Autonomous AI Security Gateway: Automated Incident Response

## 🚀 Project Overview
This project demonstrates a closed-loop **Automated Incident Response (AIR)** system designed to protect a Generative AI gateway. By integrating **Wazuh SIEM** with a custom **Ollama** (Llama 3) environment, I established a real-time detection mechanism for prompt injection and jailbreak attempts that orchestrates kernel-level containment in under 1 second.

---

## 🌐 Network Architecture
I designed a three-node environment to simulate a real-world SOC workflow:

![Network Architecture](Ai%20Defense%20.drawio.png)

| Node | OS | Roles & Services | IP Address |
| :--- | :--- | :--- | :--- |
| **Attacker** | Kali Linux | Attack Simulation / Jailbreak Attempts | `192.168.10.250` |
| **Defender** | Ubuntu Server | AI Gateway (Python), Ollama, Wazuh Agent, Iptables | `192.168.10.7` |
| **SIEM** | Wazuh Manager | Log Analysis, Alerting, & Active Response | `192.168.10.6` |

---

## 🛠️ Technical Implementation

### 1. The AI Security Gateway
I developed a custom Python middleware using **Flask** to interface with the Ollama LLM. This gateway performs **JSON forensic logging**, capturing every interaction (including the attacker's source IP and raw prompt) to `/var/log/ai_security.log` for SIEM ingestion.

### 2. Threat Detection logic
I authored custom **Wazuh XML rules** using regex to identify malicious patterns such as "bypass," "guardrail," and "ignore instructions." 
* **Rule ID:** `100006`
* **Level:** 10 (Critical)

### 3. Automated Containment
Upon a Rule 100006 match, the Wazuh Manager triggers an **Active Response**. The SIEM programmatically updates the host's **iptables** to `DROP` all traffic from the offender's IP, achieving near-instant containment.

---

## 📊 Evidence of Containment

### 1. The Detection
The Wazuh Dashboard captures the jailbreak attempt and identifies the malicious intent immediately.
![Wazuh Alert](SIEM%20Block%20LOG.png)

### 2. The Enforcement
Verification that the **Active Response** script successfully updated the kernel firewall to isolate the attacker.
![Firewall Enforcement](SIEM%20Proof%20block%20IP.png)

---

## 🎓 Skills Demonstrated
* **Security Orchestration, Automation, and Response (SOAR)**
* **SIEM Administration & Rule Development (Wazuh)**
* **Network Defense & Firewall Management (Iptables)**
* **Python for Security Automation**
* **Log Analysis & JSON Normalization**

---
*Developed by Orel Vaknin as part of Cybersecurity & Information Assurance studies at Western Governors University (WGU).*
