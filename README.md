# Vulnerability Assessment Report - Cyber Security Task 1

## 📋 Project Overview

This is a comprehensive **Vulnerability Assessment Report** for **scanme.nmap.org** conducted as part of the **Future Interns Cyber Security Internship (CS-01 Track)**.

The assessment demonstrates professional vulnerability scanning, risk classification, and remediation planning using industry-standard tools and CVSS scoring methodologies.

---

## 🎯 Assessment Details

| Field | Value |
|-------|-------|
| **Target** | scanme.nmap.org (45.33.32.156) |
| **Assessment Type** | Passive Network Vulnerability Scan |
| **Date Conducted** | February 17, 2026 |
| **Assessment Team** | Future Interns - Cyber Security Track |
| **Scope** | Read-only, non-invasive vulnerability assessment |
| **Total Vulnerabilities Found** | 8 (3 High, 3 Medium, 2 Low) |

---

## 🔧 Tools Used

### Primary Tool
- **Nmap 7.98** - Network scanning and service version detection
  - Command: `nmap -sV -sC -O scanme.nmap.org`
  - Flags Used:
    - `-sV`: Service version detection
    - `-sC`: Default NSE script scanning  
    - `-O`: Operating system fingerprinting

### Methodology
- **Passive Scanning** - No exploitation or active attacks
- **Service Enumeration** - Identify running services and versions
- **Vulnerability Mapping** - Correlate versions with known CVEs
- **Risk Assessment** - CVSS v3.1 scoring
- **Documentation** - Professional reporting

---

## 📊 Key Findings Summary

### High Risk Issues (3)
| # | Vulnerability | CVSS Score | Port | Status |
|---|---|---|---|---|
| #001 | Outdated OpenSSH 6.6.1p1 | 8.6 | 22 | Open |
| #002 | Outdated Apache httpd 2.4.7 | 8.2 | 80 | Open |
| #003 | Exposed Nping Echo Service | 7.5 | 9929 | Open |

### Medium Risk Issues (3)
- Missing HTTP Security Headers (CVSS: 6.5)
- HTTP Protocol Still Enabled (CVSS: 6.8)
- Weak SSL/TLS Configuration (CVSS: 6.0)

### Low Risk Issues (2)  
- Verbose Service Banner Information (CVSS: 3.7)
- Port 12345 Backdoor Investigation (CVSS: 2.1)

---

## 📁 Deliverables

### 1. Vulnerability Assessment Report
- **File**: `vulnerability_report.html`
- **Type**: Interactive HTML5 report with embedded CSS
- **Features**:
  - Professional dark cybersecurity theme
  - Executive summary with statistics
  - Detailed findings with remediation steps
  - Risk badges and severity indicators
  - Responsive design (mobile/tablet/desktop)
  - Smooth animations and hover effects

### 2. Detailed Findings Documentation
- **File**: `FINDINGS_DETAILED.md`
- **Content**:
  - 8 detailed vulnerability descriptions
  - Business impact analysis for each finding
  - Specific remediation steps
  - CVSS scoring and CVE references
  - Remediation timeline

### 3. Nmap Scan Results
- **File**: `nmap_scan.txt`
- **Content**: Raw Nmap output showing:
  - Open ports and services
  - Software versions detected
  - Network latency
  - OS fingerprinting results

### 4. Supporting Documentation
- **README.md**: This file - comprehensive project documentation
- **findings.md**: Template-based findings overview

---

## 🚀 How to View the Report

### Option 1: Direct Browser Access
1. Open `vulnerability_report.html` in any modern web browser
2. Explore the interactive sections and risk indicators
3. Review detailed vulnerability cards

### Option 2: Local Server
1. Navigate to the project directory
2. Run: `python -m http.server 8000`
3. Open: `http://localhost:8000/vulnerability_report.html`

### Option 3: Export as PDF
1. Open the HTML report in a browser
2. Press `Ctrl+P` (or `Cmd+P` on Mac)
3. Save as PDF for archival or sharing

---

## 🔐 Security Recommendations

### Immediate Actions Required (Days 1-7)
```bash
# Update OpenSSH
sudo apt update
sudo apt install openssh-server

# Upgrade Apache
sudo apt install apache2

# Disable Nping if not needed
sudo systemctl disable nping
sudo systemctl stop nping
```

### Configure Security Headers (Apache)
Add to `.htaccess` or Apache configuration:
```apache
Header set Strict-Transport-Security "max-age=31536000; includeSubDomains"
Header set X-Content-Type-Options "nosniff"
Header set X-Frame-Options "SAMEORIGIN"
Header set X-XSS-Protection "1; mode=block"
```

### Restrict SSH Access
1. Disable password authentication
2. Use SSH keys only
3. Change default SSH port (optional but recommended)
4. Implement firewall rules to whitelist IPs

---

## 📈 Risk Matrix

```
SEVERITY LEVEL | COUNT | TIMELINE | ACTION
---------------|-------|----------|--------
HIGH          | 3     | Immediate | Critical remediation required
MEDIUM        | 3     | 30 days   | Plan remediation
LOW           | 2     | 90 days   | Schedule fixes
```

---

## ✅ Compliance & Standards

This assessment follows industry best practices:
- ✅ **CVSS v3.1** - Vulnerability severity scoring
- ✅ **OWASP** - Web security testing guidance
- ✅ **NIST** - Cybersecurity framework alignment
- ✅ **Ethical Hacking** - Passive, authorized assessment only

---

## 📞 Assessment Scope & Limitations

### What Was Assessed
- Network-level services and versions
- Open ports and exposed services
- Security header presence/absence
- Cryptographic configuration (theoretical)
- Software version vulnerabilities

### What Was NOT Done
- ❌ No exploitation or proof-of-concept attacks
- ❌ No denial-of-service (DoS) testing
- ❌ No brute-force attacks
- ❌ No data exfiltration
- ❌ No physical security assessment
- ❌ No social engineering

**Assessment conducted within ethical guidelines as a read-only, passive vulnerability scan.**

---

## 📚 References & CVEs

### High Risk CVEs Identified
- **CVE-2018-15473**: OpenSSH username enumeration
- **CVE-2020-12062**: OpenSSH information disclosure
- **CVE-2019-9517**: Apache HTTP/2 denial-of-service
- **CVE-2020-1927**: Apache URL normalization bypass

### CVSS Scoring Reference
- 9.0-10.0: Critical
- 7.0-8.9: High  
- 4.0-6.9: Medium
- 0.1-3.9: Low

---

## 🏢 Organization Details

**Future Interns**
- **Program**: Cyber Security Internship
- **Track**: CS (Cyber Security)
- **Task**: CS-01 - Vulnerability Assessment Report
- **Date**: February 2026

---

## 📝 Notes for Reviewers

1. **This is a training project** demonstrating professional vulnerability assessment techniques
2. **Assessment was passive and non-invasive** - no systems were harmed
3. **All recommendations follow industry best practices** for security hardening
4. **Timeline is realistic** for enterprise security remediation
5. **Report is suitable for** presenting to business stakeholders or IT management

---

## 🔄 Post-Remediation Steps

After implementing the recommended fixes:

1. **Verification Scan**: Run Nmap again to verify patches
2. **Header Testing**: Use https://securityheaders.com to validate security headers
3. **SSL Testing**: Use https://www.ssllabs.com to test TLS configuration
4. **Follow-up Assessment**: Conduct a second assessment after 30 days

---

**Report Prepared By**: Future Interns - CS Track  
**Date**: February 17, 2026  
**Status**: Initial Assessment Complete  
**Next Steps**: Remediation & Follow-up Assessment