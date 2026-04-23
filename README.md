# 🔍 Network Steganography-Based Data Exfiltration Detection

A hybrid **Machine Learning + Rule-Based** system to detect covert data exfiltration over network protocols including DNS, ICMP, and HTTP.

---

## 📌 Project Overview

This project detects **network steganography** — a technique where attackers hide and exfiltrate sensitive data inside normal-looking network traffic. The system analyzes PCAP (packet capture) files and flags suspicious activity using both rule-based logic and a trained Random Forest ML model.

### Detected Attack Types:
| Attack Type | Protocol | Detection Method |
|---|---|---|
| DNS Tunneling | DNS | Rule-based + ML |
| Payload Exfiltration | ICMP | Rule-based + ML |
| Header Exfiltration | HTTP | Rule-based + ML |

---

## 🗂️ Project Structure

```
Network_Stego_proj/
├── data/
│   ├── dataset.csv          # Extracted features dataset
│   ├── dns_exfil.pcap       # DNS exfiltration traffic
│   ├── http_exfil.pcap      # HTTP exfiltration traffic
│   ├── icmp_exfil.pcap      # ICMP exfiltration traffic
│   └── normal.pcap          # Normal (benign) traffic
├── demo/
│   └── run_detector.py      # Main detection script
├── features/
│   └── extract_features.py  # Feature extraction from PCAPs
├── ml/
│   ├── train_model.py        # ML model training script
│   ├── model.joblib          # Trained Random Forest model
│   ├── encoder.joblib        # Label encoder
│   └── feature_cols.joblib  # Feature column list
└── rules/
    └── rule_engine.py        # Rule-based detection logic
```

---

## ⚙️ How It Works

### 1. Feature Extraction
Raw PCAP files are parsed using **PyShark** to extract:
- Protocol type
- Packet length & payload length
- DNS query length
- HTTP header length
- Time delta between packets
- Rule flag (hybrid feature)

### 2. Rule-Based Detection
Hard-coded thresholds to catch obvious exfiltration:
- ICMP packet size > **1000 bytes**
- DNS query length > **50 characters**
- HTTP header size > **200 bytes**

### 3. ML-Based Detection
A **Random Forest Classifier** trained on labeled PCAP data (normal vs exfiltration) with features extracted from real traffic captures.

---

## 🚀 How to Run

### Prerequisites
```bash
pip install pandas scikit-learn joblib pyshark
```

### Step 1: Extract Features (if running on new PCAPs)
```bash
python features/extract_features.py
```

### Step 2: Train the Model
```bash
python ml/train_model.py
```

### Step 3: Run the Detector
```bash
python demo/run_detector.py
```

### Sample Output
```
🚨 NETWORK DATA EXFILTRATION REPORT
=======================================
Total Packets Analyzed : 15000

1) DNS Tunneling Detected ✅
   - Suspicious DNS Queries : 342 packets
   - Avg Query Length       : 63.4
   - Detection Logic        : Rule-based + ML

2) ICMP Payload Exfiltration Detected ✅
   - Large ICMP Packets     : 198 packets
   - Avg Payload Size       : 1124.7 bytes
   - Detection Logic        : Rule-based

Overall Assessment:
---------------------------------------
⚠ Covert data exfiltration activity CONFIRMED
Multiple protocols used for data leakage

Recommended Actions:
---------------------------------------
- Inspect compromised host
- Block suspicious endpoints
- Preserve traffic for forensic analysis
```

---

## 🛠️ Technologies Used

- **Python 3.11**
- **PyShark** — PCAP packet parsing
- **Scikit-learn** — Random Forest Classifier
- **Pandas** — Data processing
- **Joblib** — Model serialization

---

## 🔐 Security Concepts Covered

- Network Steganography
- Covert Channel Detection
- DNS Tunneling
- ICMP Exfiltration
- HTTP Header Abuse
- Hybrid Intrusion Detection (Rules + ML)

---

## 👩‍💻 Author

**Zoha Shakeel**  
BS Information Technology — NUTECH, Islamabad  
Cybersecurity & Software Development

---

## 📝 License

© 2025 — All Rights Reserved
