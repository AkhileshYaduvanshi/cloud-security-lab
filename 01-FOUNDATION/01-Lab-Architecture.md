# Lab Architecture  
## CipherCare Cloud Security Lab  
### High-Level Design (HLD)

---

# 1. Executive Summary

This document defines the high-level architecture for the CipherCare Cloud Security Lab.

CipherCare Solutions is a simulated healthcare organization operating a web-based patient management platform built using:

- Django (Backend)
- React (Frontend)
- PostgreSQL (Database)

The architecture follows a secure 3-tier model designed to simulate real-world healthcare infrastructure while operating within stateless cloud constraints (KodeKloud playground).

The primary objective of this design is:

- Secure-by-design infrastructure
- Controlled attack simulation
- Real-time detection engineering
- Hybrid cloud + home lab SOC integration
- Data persistence strategy across ephemeral sessions

---

# 2. Business Context

CipherCare Solutions is a fictional healthcare provider handling:

- Patient PII (Personally Identifiable Information)
- PHI (Protected Health Information)
- Login & Authentication flows
- Mock payment transactions
- Medical API workflows

The system must:

- Protect sensitive data
- Prevent unauthorized access
- Detect malicious behavior
- Maintain reproducibility under cloud reset conditions

---

# 3. Architectural Overview – 3 Tier Model

The system follows a traditional secure 3-tier architecture.

---

## Tier 1 – Presentation Layer (Public Zone)

Components:

- Application Load Balancer (ALB)
- Web Application Firewall (WAF)

Location:

- Public Subnets

Responsibilities:

- Accept user traffic
- Enforce WAF rules
- TLS termination
- Forward traffic to application tier

Security Controls:

- WAF rules for OWASP Top 10
- HTTPS only
- Restricted inbound ports (80/443 only)
- No direct EC2 public exposure

---

## Tier 2 – Application Layer (Private Zone)

Components:

- EC2 instances running CipherCare application
- Django backend
- React frontend

Location:

- Private Subnets

Responsibilities:

- Business logic execution
- Authentication handling
- API processing
- Payment workflow handling

Security Controls:

- No public IP
- Access only via ALB
- Security group restricted to ALB only
- IAM role-based access
- Secrets stored securely (Later Vault integration)

---

## Tier 3 – Data Layer (Isolated Zone)

Components:

- RDS PostgreSQL

Location:

- Isolated Private Subnet (No Internet Route)

Responsibilities:

- Store patient records
- Store login credentials (hashed)
- Store mock payment data

Security Controls:

- No public accessibility
- Encryption at rest enabled
- Encrypted connections enforced
- Access restricted to Application Security Group only
- Automated snapshot backups

---

# 4. Network & IP Strategy

Cloud VPC Range:

```

10.1.0.0/16

```

Home Lab Range:

```

172.16.0.0/16

```

No CIDR overlap is allowed.

Proposed Segmentation:

- 10.1.1.0/24 – Public Subnet A
- 10.1.2.0/24 – Public Subnet B
- 10.1.10.0/24 – Application Private Subnet
- 10.1.20.0/24 – Database Isolated Subnet

Future:

- Site-to-Site VPN connection between to Home SOC Lab

This enables hybrid SOC monitoring and log ingestion.

---

# 5. Compliance & Security Alignment

This architecture is aligned conceptually with:

## HIPAA (Healthcare Security Thinking)

- Encryption at rest (RDS)
- Access control policies
- Audit logging enabled
- Restricted database exposure
- Role-based identity management

## PCI-DSS (Payment Awareness)

- Secure handling of mock payments
- No sensitive data stored in plaintext
- Segregated payment logic
- Log review and monitoring

## OWASP Top 10

Protection focus against:

- Injection attacks
- Broken authentication
- Security misconfiguration
- Sensitive data exposure
- Logging & monitoring failures

WAF rules and red-team simulations validate these controls.

---

# 6. Data Persistence Strategy (Hybrid Retention Model)

Because KodeKloud sessions expire every 2–3 hours:

Cloud infrastructure is stateless.

However, patient data persistence is required for continuity.

---

## Retention Design

Primary Cloud DB → Export → Local Proxmox Storage → Re-import Next Session

Workflow:

1. End of session:
   - Run pg_dump on RDS.
   - Export db_dump.sql.
   - Transfer to local Proxmox storage.

2. Playground expires.

3. New session:
   - Recreate infrastructure.
   - Restore database.
   - Import db_dump.sql.
   - Resume from last state.

4. After modifications:
   - Re-export updated dump.
   - Backup again locally.

This creates a dual-layer persistence model:

- Local secure storage (Proxmox)
- Ephemeral cloud rebuild

This simulates disaster recovery and backup validation practice.

---

# 7. Threat Surface Overview

Major attack surfaces:

- Public-facing ALB
- Application authentication system
- IAM misconfiguration
- Database access controls
- Storage misconfigurations
- Secrets exposure

These surfaces will be tested in the RED-TEAM phase.

Detection coverage will be validated in the BLUE-TEAM phase.

---

# 8. Logging & Monitoring Strategy

Planned logging:

- CloudTrail (API audit)
- VPC Flow Logs
- ALB access logs
- Application logs
- RDS logs

Future integration:

- Log forwarding to Wazuh (Home Lab)
- Hybrid SOC monitoring
- Correlation between cloud + on-prem events

---

# 9. Design Constraints

- KodeKloud playground expires every 2–3 hours.
- Low resource usage (t3.micro / small instances).
- No production data.
- Manual-first deployment before IaC automation.
- Security understanding prioritized over speed.

---

# 10. Future Evolution

After full manual mastery:

- Convert to Terraform
- Implement remote state backend
- Integrate Vault
- Harden CI/CD
- Introduce containerization
- Add auto-scaling
- Multi-AZ high availability

But security validation always remains primary.

---

# Conclusion

The CipherCare Cloud Security Lab architecture is designed to:

- Simulate enterprise healthcare infrastructure
- Enforce secure network segmentation
- Enable adversarial testing
- Support hybrid SOC integration
- Survive stateless cloud constraints
- Practice real-world cloud security engineering

This architecture is not designed for scale.

It is designed for understanding.
```
