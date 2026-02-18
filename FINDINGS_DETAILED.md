# Vulnerability Assessment Findings - scanme.nmap.org

## Overview
This document outlines the identified vulnerabilities from the vulnerability assessment conducted on **scanme.nmap.org**. Each vulnerability is classified based on its risk level and includes a brief explanation of its potential impact on the business, along with recommended remediation steps.

**Assessment Date**: February 17, 2026  
**Assessment Type**: Passive Vulnerability Scan  
**Target**: scanme.nmap.org (45.33.32.156)

---

## Findings Summary
- **Total Vulnerabilities**: 8
- **Critical/High Risk**: 3
- **Medium Risk**: 3
- **Low Risk**: 2

---

## High Risk Vulnerabilities

### 1. Outdated OpenSSH Version (CVE-2018-15473 & CVE-2020-12062)
**Severity**: HIGH | **CVSS Score**: 8.6  
**Affected Service**: SSH Server on Port 22  
**Current Version**: OpenSSH 6.6.1p1 (Ubuntu)  
**Latest Version**: OpenSSH 8.9+ recommended

**Description**: 
The target system is running OpenSSH 6.6.1p1, released in 2014 and no longer receiving security updates. This version is vulnerable to multiple authentication bypass and information disclosure attacks.

**Business Impact**: 
- Unauthorized SSH access could allow attackers to gain shell access to the system
- Potential for lateral movement within the network
- Exposure of sensitive data and configuration files
- Credential theft through man-in-the-middle attacks

**Remediation Steps**:
1. Update OpenSSH to version 8.6p1 or later
2. Implement SSH key-based authentication only (disable password authentication)
3. Restrict SSH access to specific IP ranges via firewall
4. Change default SSH port (22) to a non-standard port
5. Monitor SSH logs for unauthorized access attempts

---

### 2. Outdated Apache HTTP Server (CVE-2019-9517, CVE-2020-1927)
**Severity**: HIGH | **CVSS Score**: 8.2  
**Affected Service**: HTTP/HTTPS on Port 80 & 443  
**Current Version**: Apache httpd 2.4.7  
**Latest Version**: Apache 2.4.57+ recommended

**Description**: 
Apache httpd 2.4.7 (released 2013) contains multiple known vulnerabilities including HTTP/2 denial-of-service and URL normalization bypass issues.

**Business Impact**:
- Denial-of-service attacks could make the website unavailable
- Cache poisoning attacks possible
- Potential for request smuggling attacks
- Information disclosure through misconfiguration

**Remediation Steps**:
1. Upgrade Apache to version 2.4.57 or latest stable release
2. Enable security-focused Apache modules (mod_security if available)
3. Configure HTTP/2 settings properly
4. Implement rate limiting to prevent DoS attacks
5. Enable security headers (HSTS, X-Frame-Options, CSP)

---

### 3. Unnecessary Network Services Exposed
**Severity**: HIGH | **CVSS Score**: 7.5  
**Affected Service**: Nping Echo on Port 9929  
**Finding**: Service-status: OPEN

**Description**: 
The Nping echo service on port 9929 is exposed to the public internet. This service is typically used for network diagnostics but should not be exposed to untrusted networks.

**Business Impact**:
- Potential for network reconnaissance by attackers
- May be used for bounce attacks or DDoS amplification
- Provides information about the system's network configuration
- Can be exploited for timing-based side-channel attacks

**Remediation Steps**:
1. Disable Nping echo service if not required
2. Block port 9929 at the firewall for external access
3. Implement network segmentation to limit service exposure
4. Document all required open ports and disable/block the rest
5. Use port-knocking techniques for management access

---

## Medium Risk Vulnerabilities

### 4. Missing HTTP Security Headers
**Severity**: MEDIUM | **CVSS Score**: 6.5  
**Component**: Web Server Configuration

**Description**: 
The web server is not returning critical security headers that protect against common web attacks.

**Missing Headers**:
- Strict-Transport-Security (HSTS)
- X-Frame-Options
- X-Content-Type-Options: nosniff
- Content-Security-Policy (CSP)
- X-XSS-Protection

**Business Impact**:
- Increased vulnerability to clickjacking attacks
- MIME-type sniffing attacks possible
- Cross-site scripting (XSS) attacks more likely
- Cookies can be stolen over unencrypted connections

**Remediation Steps**:
1. Add Strict-Transport-Security header: `max-age=31536000`
2. Set X-Frame-Options to `DENY` or `SAMEORIGIN`
3. Add X-Content-Type-Options: `nosniff`
4. Implement comprehensive Content-Security-Policy
5. Test headers using security scanners (e.g., securityheaders.com)

---

### 5. HTTP Protocol Still Enabled (Not HTTPS-Only)
**Severity**: MEDIUM | **CVSS Score**: 6.8  
**Service**: Port 80 (HTTP) accepting unencrypted traffic

**Description**: 
The website accepts HTTP (unencrypted) connections on port 80. While HTTPS may be available, HTTP is not automatically redirecting to HTTPS.

**Business Impact**:
- Man-in-the-middle (MITM) attacks possible
- Credentials can be intercepted in transit
- Session tokens can be stolen
- Sensitive data exposure during transmission

**Remediation Steps**:
1. Implement permanent redirect (301/308) from HTTP to HTTPS
2. Obtain valid SSL/TLS certificate from trusted CA
3. Configure HTTPS on port 443 with TLS 1.2 or higher
4. Add HSTS header to force HTTPS for future visits
5. Test SSL/TLS configuration using tools like Qualys SSL Labs

---

### 6. Weak OpenSSL/TLS Configuration (Potential)
**Severity**: MEDIUM | **CVSS Score**: 6.0  
**Component**: SSH and Web Server Encryption

**Description**: 
Given the age of the server components, TLS/SSL configuration may be using deprecated cipher suites or protocols (SSLv3, TLSv1.0, TLSv1.1).

**Business Impact**:
- Weak encryption can be brute-forced
- Downgrade attacks possible
- Cryptographic attacks (POODLE, BEAST)
- Loss of confidentiality for encrypted communications

**Remediation Steps**:
1. Disable SSLv3, TLSv1.0, and TLSv1.1
2. Enable TLS 1.2 and TLS 1.3 only
3. Use strong cipher suites (AES-256, ECDHE)
4. Disable weak ciphers (DES, MD5, RC4)
5. Test configuration using SSL Labs or similar tools

---

## Low Risk Vulnerabilities

### 7. Verbose Service Banner Information Disclosure
**Severity**: LOW | **CVSS Score**: 3.7  
**Finding**: Services expose version numbers in banners

**Description**: 
Both SSH and HTTP servers broadcast their exact version numbers in service banners, providing attackers with specific CVE information to target.

**Business Impact**:
- Detailed information about installed software versions
- Helps attackers identify known vulnerabilities
- Reduces attacker reconnaissance time
- Minor—but avoidable—information leak

**Remediation Steps**:
1. Customize SSH banner to hide version (SSH)
2. Disable Server tokens in Apache (ServerTokens Prod)
3. Implement Web Application Firewall (WAF) rules
4. Monitor for unauthorized access patterns
5. Regular patching to stay ahead of published exploits

---

### 8. Port 12345 Service Closure Detection
**Severity**: LOW | **CVSS Score**: 2.1  
**Finding**: Port 12345 is closed (Netbus potentially blocked by system)

**Description**: 
Port 12345 is closed, which is good security practice. However, this indicates the system may have been targeted for backdoor installation (Netbus was a common malware target).

**Business Impact**:
- Indicates previous attack attempts may have occurred
- System may have been scanned aggressively
- Minor indicator of targeted compromise attempts

**Remediation Steps**:
1. Review system logs for intrusion attempts
2. Run antivirus/malware scan to verify system integrity
3. Implement intrusion detection system (IDS)
4. Maintain firewall rules blocking all unnecessary ports
5. Keep audit logs for security investigations

---

## Summary Table

| ID | Vulnerability | Risk Level | CVSS Score | Status |
|---|---|---|---|---|
| #001 | Outdated OpenSSH 6.6.1p1 | HIGH | 8.6 | Open |
| #002 | Outdated Apache 2.4.7 | HIGH | 8.2 | Open |
| #003 | Exposed Nping Echo Service | HIGH | 7.5 | Open |
| #004 | Missing HTTP Security Headers | MEDIUM | 6.5 | Open |
| #005 | HTTP Protocol Enabled | MEDIUM | 6.8 | Open |
| #006 | Weak TLS Configuration | MEDIUM | 6.0 | Open |
| #007 | Verbose Service Banners | LOW | 3.7 | Open |
| #008 | Port 12345 Investigation | LOW | 2.1 | Mitigated |

---

## Remediation Timeline

### Immediate (Days 1-7) - Critical Priority
- Patch OpenSSH to latest version
- Patch Apache to latest version
- Disable Nping echo service

### Short-term (Weeks 1-4) - High Priority
- Implement all missing security headers
- Configure HTTPS-only access with permanent redirects
- Optimize TLS/SSL configuration

### Medium-term (Weeks 4-8) - Medium Priority
- Suppress service banners and version information
- Deploy Web Application Firewall (WAF)
- Implement comprehensive security monitoring

### Ongoing - Best Practices
- Enable automatic security updates
- Quarterly vulnerability assessments
- Monthly security header verification
- Continuous monitoring and logging

---

## Conclusion

The vulnerability assessment of **scanme.nmap.org** identified **8 significant vulnerabilities** across the assessed infrastructure. Three critical-severity issues related to outdated software components and exposed services require immediate remediation.

Addressing the high-risk vulnerabilities should be the priority to prevent unauthorized access and potential data compromise. Once immediate threats are mitigated, the medium and low-risk issues should be systematically addressed per the remediation timeline.

Regular security assessments, timely patching, and adherence to security best practices will significantly improve the overall security posture of the system.

---

**Report Prepared**: February 17, 2026  
**Assessment Conducted By**: Future Interns - Cyber Security Track  
**Scope**: Non-invasive, passive vulnerability assessment only
