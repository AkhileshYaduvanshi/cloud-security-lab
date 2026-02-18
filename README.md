# Cloud Security Lab  
### Healthcare Infrastructure Simulation

---

## Project Overview

Cloud Security Lab is a security-focused cloud engineering project where I design, build, attack, detect, and harden a simulated healthcare infrastructure.

This lab is built around a fictional medical organization called **CipherCare Solutions**, operating a patient management system with sensitive healthcare data (PII, authentication systems, and payment workflows).

The main objective of this lab is:

> To deeply understand Cloud Security by building real-world architecture, breaking it, and learning how to defend it.

This project is executed using KodeKloud playgrounds, which reset every 2–3 hours. Because of this constraint, the lab is designed to be **stateless and reproducible**.

---

## Core Learning Goal

The primary focus of this project is:

- Cloud Security Engineering  
- Threat Modeling  
- Detection Engineering  
- Red Team vs Blue Team Simulation  
- Hybrid Cloud ( Enterpirse Home Lab)

Infrastructure and DevOps tools are secondary.  
Security understanding comes first.

---

## Simulated Organization: CipherCare Solutions

CipherCare Solutions is a fictional healthcare provider with:

- Web-based medical application
- Patient profiles (PII data)
- Login & authentication workflows
- Payment integration components
- Backend API services
- PostgreSQL database

The infrastructure is designed to simulate a real production healthcare environment while keeping resource usage minimal (t3.micro/small instances).

---

## Core Philosophy

This lab follows five guiding principles:

1. **Security First** – Infrastructure exists to be secured and attacked.
2. **Manual Before Automation** – Understand deeply before using IaC.
3. **Attack What You Build** – Every design must be tested adversarially.
4. **Stateless by Design** – Must survive playground resets.
5. **Hybrid Thinking** – Cloud and on-prem security must integrate.

---

## Learning Phases

### Phase 1 – Foundation
- Define high-level architecture
- Plan IP addressing
- Design IAM roles

### Phase 2 – Infrastructure
- Build secure VPC
- Create public/private subnet separation
- Deploy EC2 and RDS
- Deploy healthcare application

### Phase 3 – Security Engineering
- Create threat model
- Enable CloudTrail & logging
- Configure monitoring
- Design backup strategy
- Map compliance (HIPAA-style thinking, PCI awareness, OWASP Top 10)

### Phase 4 – Red Team
- SQL Injection simulation
- Broken authentication testing
- IAM privilege escalation testing
- Cloud misconfiguration exploitation
- Data exfiltration simulation

### Phase 5 – Blue Team
- Log analysis
- Detection rule creation
- Incident investigation
- Threat hunting exercises
- Correlation with SIEM (Wazuh home lab)

### Phase 6 – Hybrid Integration
- VPN architecture between cloud and home lab
- Centralized log monitoring
- Hybrid SOC simulation

### Phase 7 – DevSecOps (Later Stage)
- Convert manual builds to Terraform
- Introduce secret management (Vault)
- CI/CD pipeline hardening
- Infrastructure as Code automation

---

## Security Focus Areas

This lab specifically emphasizes:

- Network segmentation
- Least privilege IAM
- Secure secret handling
- Encryption at rest and in transit
- Misconfiguration detection
- Cloud-native threat modeling
- Log-based detection engineering

Security is not an afterthought here.  
Security is the foundation.

---

## Why This Project Exists

This project is built to:

- Develop real-world cloud security skills
- Simulate enterprise healthcare architecture
- Understand attacker mindset
- Practice defensive detection
- Bridge cloud and on-prem security environments

The goal is not certification preparation.  
The goal is practical cloud security mastery.

---

## Disclaimer

This lab is for educational and internal simulation purposes only.

All attack scenarios are performed in controlled sandbox environments.

No real healthcare data or production systems are involved.

---

### Final Note

This repository represents a continuous learning journey in Cloud Security Engineering.

Every architecture decision, attack simulation, and detection strategy is documented to build deep understanding — not just surface-level tool knowledge.
```
