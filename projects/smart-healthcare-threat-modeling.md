# Smart Healthcare Mobile Application: Threat Modeling Case Study

> Academic coursework | Security design and risk assessment  
> Scope: A hypothetical healthcare application. This document describes a threat-modeling exercise, not a production penetration test or a deployed security product.

## Overview

The assessment models a mobile healthcare application used by patients and doctors. The system supports account access, medical records, appointments, video consultations, digital prescriptions, wearable-device data, payments, and notifications.

The goal was to understand the system, identify trust boundaries and important assets, map plausible threats with STRIDE, assess risk, and propose security controls.

## System scope

| Component | Responsibility |
|---|---|
| Patient and doctor clients | User access, appointments, records, consultations, prescriptions |
| Backend and API Gateway | Authentication, authorization, business logic, and API requests |
| Database | User profiles, appointments, and structured application data |
| Cloud storage | Medical documents and larger patient files |
| Payment gateway | External processing of consultation payments |
| Wearable APIs | External health metrics such as heart rate and steps |
| Notification service | Appointment and medication reminders |

## Simplified data-flow view

\`\`\`mermaid
flowchart LR
    P[Patient] --> APP[Mobile Application]
    D[Doctor] --> APP
    APP --> GW[API Gateway]
    GW --> AUTH[Authentication and Authorization]
    GW --> REC[Records and Appointments]
    GW --> IOT[Wearable Data Sync]
    GW --> PAY[Payment Integration]
    REC --> DB[(Application Database)]
    REC --> CLOUD[(Medical File Storage)]
    IOT <--> W[Wearable API]
    PAY <--> PG[Payment Provider]
    GW --> N[Notification Service]
\`\`\`

The detailed report included Level 0 and Level 1 DFDs. The Level 1 model highlighted trust boundaries between client devices, internal backend services, cloud storage, and external third-party services.

## Asset inventory

The assessment documented eight main assets:

1. Doctor credentials and authentication tokens.
2. Medical records and patient history.
3. Digital prescriptions and audit records.
4. Wearable telemetry data.
5. Video consultation streams.
6. Payment tokens and transaction identifiers.
7. Mobile client applications.
8. Backend API Gateway.

## STRIDE threat scenarios

| STRIDE category | Scenario | Main affected assets |
|---|---|---|
| Spoofing | Replaying a valid session token to impersonate a doctor | Doctor credentials, video stream |
| Tampering | Modifying wearable health data in transit | Wearable telemetry |
| Repudiation | Changing a prescription without reliable audit evidence | Prescriptions, audit logs |
| Information disclosure | Exposing patient files through misconfigured cloud-storage permissions | Medical records |
| Denial of service | Flooding appointment-booking APIs with automated requests | Backend API Gateway |
| Elevation of privilege | Exploiting an insecure direct object reference to access another patient's records | Medical records, mobile client |

## Risk assessment

The coursework used qualitative likelihood and impact ratings.

| Threat | Likelihood | Impact | Assessed risk |
|---|---:|---:|---:|
| Session-token spoofing | Medium | High | High |
| Wearable-data tampering | Medium | Medium | Medium |
| Missing prescription audit logs | Low | High | Medium |
| Cloud-storage exposure | Low | High | High |
| API denial of service | High | High | Critical |
| Patient-to-patient IDOR | Medium | High | High |

## Proposed security controls

- Enforce multi-factor authentication for sensitive accounts and secure session handling.
- Protect wearable data in transit with TLS and validate data at the backend.
- Maintain tamper-resistant audit trails for prescription and medical-record changes.
- Apply least-privilege permissions to cloud storage and medical records.
- Add API rate limiting and traffic controls to reduce denial-of-service risk.
- Enforce server-side role and object-level authorization checks to prevent IDOR.

## Work delivered

- System scope and component analysis.
- Data Flow Diagrams at Level 0 and Level 1.
- Asset identification.
- STRIDE threat scenarios.
- Qualitative risk assessment.
- Security mitigation recommendations.

## Scope and limitations

This was a design-level academic assessment based on a hypothetical application scenario. The findings are modeled risks and proposed controls. No live application, production infrastructure, or real patient data were tested.
