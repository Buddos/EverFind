# Non-Functional Requirements Document
## Hardware-Based Persistent Tracking System

**Document Version:** 1.0  
**Date:** March 2026  
**Status:** Final

---

## 1. Introduction

### 1.1 Purpose
This document specifies the non-functional requirements for the Hardware-Based Persistent Tracking System, including performance, security, scalability, reliability, and other quality attributes.

### 1.2 Scope
This document covers performance requirements, security requirements, scalability requirements, reliability requirements, usability requirements, and compliance requirements.

---

## 2. Performance Requirements

### 2.1 Response Time
**NFR-2.1.1:** Web portal page load time shall not exceed 3 seconds on standard broadband connection (5 Mbps).

**NFR-2.1.2:** Mobile app page load time shall not exceed 2 seconds on 4G connection.

**NFR-2.1.3:** API response time shall not exceed 200ms for 95th percentile of requests.

**NFR-2.1.4:** Database query response time shall not exceed 100ms for standard queries.

**NFR-2.1.5:** ISP API lookup response time shall not exceed 5 seconds.

### 2.2 Throughput
**NFR-2.2.1:** The system shall support at least 1,000 concurrent users.

**NFR-2.2.2:** The system shall support at least 10,000 concurrent users during peak hours (Year 2).

**NFR-2.2.3:** The system shall process at least 100 device registrations per minute.

**NFR-2.2.4:** The system shall process at least 50 theft reports per minute.

**NFR-2.2.5:** The system shall process at least 1,000 ISP lookups per minute.

### 2.3 Latency
**NFR-2.3.1:** Notification delivery latency shall not exceed 2 minutes for email notifications.

**NFR-2.3.2:** Notification delivery latency shall not exceed 30 seconds for SMS notifications.

**NFR-2.3.3:** Notification delivery latency shall not exceed 5 seconds for push notifications.

**NFR-2.3.4:** Real-time location update latency shall not exceed 5 minutes.

### 2.4 Resource Utilization
**NFR-2.4.1:** Desktop agent shall consume less than 50MB of RAM during operation.

**NFR-2.4.2:** Desktop agent shall consume less than 5% CPU during idle state.

**NFR-2.4.3:** Mobile app shall consume less than 100MB of storage space.

**NFR-2.4.4:** Mobile app shall consume less than 10% CPU during idle state.

---

## 3. Security Requirements

### 3.1 Authentication & Authorization
**NFR-3.1.1:** All user authentication shall use strong cryptographic algorithms (bcrypt, Argon2).

**NFR-3.1.2:** Password hashes shall use salt with at least 12 rounds (bcrypt) or equivalent.

**NFR-3.1.3:** Session tokens shall expire after 30 minutes of inactivity.

**NFR-3.1.4:** API authentication shall use JWT tokens with expiration time of 1 hour.

**NFR-3.1.5:** Refresh tokens shall have expiration time of 7 days.

**NFR-3.1.6:** Role-based access control (RBAC) shall be enforced for all operations.

**NFR-3.1.7:** Multi-factor authentication shall be mandatory for police and admin users.

### 3.2 Data Encryption
**NFR-3.2.1:** All data in transit shall be encrypted using TLS 1.3 or higher.

**NFR-3.2.2:** All sensitive data at rest shall be encrypted using AES-256.

**NFR-3.2.3:** Encryption keys shall be managed using a Hardware Security Module (HSM).

**NFR-3.2.4:** Database encryption keys shall be rotated annually.

**NFR-3.2.5:** API keys shall be encrypted at rest.

**NFR-3.2.6:** Personally identifiable information (PII) shall be encrypted with separate keys per user.

### 3.3 Data Privacy
**NFR-3.3.1:** Personal data collection shall be minimized to only necessary information.

**NFR-3.3.2:** User consent shall be obtained before collecting personal data.

**NFR-3.3.3:** Data retention policies shall comply with GDPR and local regulations.

**NFR-3.3.4:** User data shall not be shared with third parties without explicit consent.

**NFR-3.3.5:** Police access to user data shall be logged and auditable.

**NFR-3.3.6:** Data deletion requests shall be processed within 30 days.

### 3.4 Input Validation & Output Encoding
**NFR-3.4.1:** All user inputs shall be validated against whitelist rules.

**NFR-3.4.2:** SQL injection attacks shall be prevented using parameterized queries.

**NFR-3.4.3:** Cross-site scripting (XSS) attacks shall be prevented using output encoding.

**NFR-3.4.4:** Cross-site request forgery (CSRF) attacks shall be prevented using CSRF tokens.

**NFR-3.4.5:** File uploads shall be validated for type, size, and content.

### 3.5 API Security
**NFR-3.5.1:** API rate limiting shall be enforced (100 requests per minute per user).

**NFR-3.5.2:** API endpoints shall require authentication and authorization.

**NFR-3.5.3:** API responses shall not expose sensitive system information.

**NFR-3.5.4:** API versioning shall be supported for backward compatibility.

### 3.6 Vulnerability Management
**NFR-3.6.1:** Security vulnerabilities shall be identified through regular penetration testing.

**NFR-3.6.2:** Penetration testing shall be conducted at least quarterly.

**NFR-3.6.3:** Critical vulnerabilities shall be patched within 24 hours.

**NFR-3.6.4:** High-severity vulnerabilities shall be patched within 7 days.

**NFR-3.6.5:** Dependency vulnerabilities shall be scanned using automated tools.

**NFR-3.6.6:** Security patches shall be applied to all systems within 30 days of release.

### 3.7 Audit & Logging
**NFR-3.7.1:** All user actions shall be logged with timestamp and user ID.

**NFR-3.7.2:** All authentication attempts (successful and failed) shall be logged.

**NFR-3.7.3:** All data access by police shall be logged and auditable.

**NFR-3.7.4:** Audit logs shall be stored separately from application data.

**NFR-3.7.5:** Audit logs shall be encrypted and immutable.

**NFR-3.7.6:** Audit logs shall be retained for at least 2 years.

---

## 4. Scalability Requirements

### 4.1 Horizontal Scalability
**NFR-4.1.1:** The system architecture shall support horizontal scaling of application servers.

**NFR-4.1.2:** Load balancing shall be implemented for distributing traffic.

**NFR-4.1.3:** Database replication shall support read replicas for scaling read operations.

**NFR-4.1.4:** Caching layer shall be implemented for reducing database load.

### 4.2 Vertical Scalability
**NFR-4.2.1:** The system shall support increasing server resources (CPU, RAM) without downtime.

**NFR-4.2.2:** Database performance shall scale with increased storage and compute resources.

### 4.3 Data Scalability
**NFR-4.3.1:** The system shall support at least 1 million registered devices.

**NFR-4.3.2:** The system shall support at least 10 million historical location records.

**NFR-4.3.3:** Database sharding shall be implemented for distributing data across multiple servers.

**NFR-4.3.4:** Archive strategy shall be implemented for old data (>2 years).

---

## 5. Reliability & Availability Requirements

### 5.1 Availability
**NFR-5.1.1:** System uptime shall be 99.9% (allowing 43.2 minutes downtime per month).

**NFR-5.1.2:** Critical services (device tracking, police portal) shall have 99.99% uptime.

**NFR-5.1.3:** Planned maintenance shall be scheduled during low-traffic periods.

**NFR-5.1.4:** Maintenance windows shall not exceed 2 hours per month.

### 5.2 Disaster Recovery
**NFR-5.2.1:** Recovery Time Objective (RTO) shall be 1 hour for critical services.

**NFR-5.2.2:** Recovery Point Objective (RPO) shall be 15 minutes for critical data.

**NFR-5.2.3:** Backup systems shall be geographically distributed.

**NFR-5.2.4:** Backup data shall be tested for recoverability at least quarterly.

**NFR-5.2.5:** Disaster recovery plan shall be documented and tested annually.

### 5.3 Redundancy
**NFR-5.3.1:** Critical services shall have redundant instances in different availability zones.

**NFR-5.3.2:** Database shall have primary-replica replication with automatic failover.

**NFR-5.3.3:** Load balancers shall be redundant and geographically distributed.

**NFR-5.3.4:** DNS services shall be provided by multiple providers.

### 5.4 Fault Tolerance
**NFR-5.4.1:** System shall gracefully handle ISP API failures.

**NFR-5.4.2:** System shall implement circuit breaker pattern for external API calls.

**NFR-5.4.3:** System shall implement retry logic with exponential backoff.

**NFR-5.4.4:** System shall provide fallback mechanisms for degraded services.

---

## 6. Usability Requirements

### 6.1 User Interface
**NFR-6.1.1:** User interface shall follow accessibility guidelines (WCAG 2.1 Level AA).

**NFR-6.1.2:** Web interface shall be responsive and work on all modern browsers.

**NFR-6.1.3:** Mobile interface shall be optimized for touch interaction.

**NFR-6.1.4:** Font size shall be at least 14px for readability.

**NFR-6.1.5:** Color contrast ratio shall be at least 4.5:1 for text.

### 6.2 User Experience
**NFR-6.2.1:** Device registration process shall be completable in less than 5 minutes.

**NFR-6.2.2:** User onboarding process shall include guided tutorials.

**NFR-6.2.3:** Error messages shall be clear and actionable.

**NFR-6.2.4:** System shall provide contextual help and documentation.

### 6.3 Internationalization
**NFR-6.3.1:** System shall support multiple languages (English, Swahili, French).

**NFR-6.3.2:** System shall support multiple currencies (USD, KES, EUR).

**NFR-6.3.3:** Date and time formats shall be localized.

**NFR-6.3.4:** Right-to-left language support shall be implemented.

---

## 7. Maintainability Requirements

### 7.1 Code Quality
**NFR-7.1.1:** Code shall follow established coding standards and conventions.

**NFR-7.1.2:** Code complexity (cyclomatic complexity) shall not exceed 10 per function.

**NFR-7.1.3:** Code documentation shall be maintained for all modules.

**NFR-7.1.4:** Code review shall be performed for all changes before deployment.

### 7.2 Testing
**NFR-7.2.1:** Unit test coverage shall be at least 80%.

**NFR-7.2.2:** Integration test coverage shall be at least 70%.

**NFR-7.2.3:** Critical paths shall have 100% test coverage.

**NFR-7.2.4:** Automated testing shall be performed on every code commit.

**NFR-7.2.5:** Performance testing shall be conducted before each release.

### 7.3 Documentation
**NFR-7.3.1:** Technical documentation shall be maintained and kept up-to-date.

**NFR-7.3.2:** API documentation shall be auto-generated from code.

**NFR-7.3.3:** Architecture diagrams shall be maintained in version control.

**NFR-7.3.4:** Deployment procedures shall be documented.

### 7.4 Monitoring & Logging
**NFR-7.4.1:** Application logs shall be centralized and searchable.

**NFR-7.4.2:** Logs shall include timestamp, severity level, and context.

**NFR-7.4.3:** System metrics shall be monitored in real-time.

**NFR-7.4.4:** Alerts shall be triggered for critical issues.

**NFR-7.4.5:** Log retention shall be at least 90 days.

---

## 8. Compliance & Legal Requirements

### 8.1 Data Protection
**NFR-8.1.1:** System shall comply with GDPR regulations.

**NFR-8.1.2:** System shall comply with CCPA regulations.

**NFR-8.1.3:** System shall comply with Kenya Data Protection Act.

**NFR-8.1.4:** Data processing agreements shall be in place with all processors.

### 8.2 Evidence Admissibility
**NFR-8.2.1:** Evidence collection and storage shall follow chain of custody procedures.

**NFR-8.2.2:** Evidence shall be digitally signed for authenticity verification.

**NFR-8.2.3:** Evidence shall include metadata for legal admissibility.

**NFR-8.2.4:** Evidence storage shall comply with legal requirements for retention.

### 8.3 Law Enforcement
**NFR-8.3.1:** System shall comply with law enforcement data access requirements.

**NFR-8.3.2:** Warrant execution procedures shall be documented.

**NFR-8.3.3:** Law enforcement access shall be logged and auditable.

**NFR-8.3.4:** System shall support evidence export in legally acceptable formats.

### 8.4 Intellectual Property
**NFR-8.4.1:** System shall respect intellectual property rights.

**NFR-8.4.2:** Third-party licenses shall be properly attributed.

**NFR-8.4.3:** Open-source components shall comply with license requirements.

---

## 9. Deployment & Infrastructure Requirements

### 9.1 Infrastructure
**NFR-9.1.1:** System shall be deployed on cloud infrastructure (AWS, Azure, or GCP).

**NFR-9.1.2:** Infrastructure shall support auto-scaling based on demand.

**NFR-9.1.3:** Infrastructure shall be managed using Infrastructure as Code (IaC).

**NFR-9.1.4:** Container orchestration shall be managed using Kubernetes.

### 9.2 Deployment
**NFR-9.2.1:** Deployment shall be automated using CI/CD pipelines.

**NFR-9.2.2:** Deployments shall support blue-green or canary deployment strategies.

**NFR-9.2.3:** Rollback capability shall be available for all deployments.

**NFR-9.2.4:** Deployment process shall be documented and tested.

### 9.3 Environment Management
**NFR-9.3.1:** Separate environments shall be maintained (development, staging, production).

**NFR-9.3.2:** Environment configuration shall be managed using environment variables.

**NFR-9.3.3:** Secrets shall be managed using secure secret management tools.

**NFR-9.3.4:** Database migrations shall be automated and version-controlled.

---

## 10. Interoperability Requirements

### 10.1 API Standards
**NFR-10.1.1:** REST APIs shall follow OpenAPI 3.0 specification.

**NFR-10.1.2:** APIs shall use JSON for request and response payloads.

**NFR-10.1.3:** APIs shall support content negotiation (JSON, XML).

**NFR-10.1.4:** APIs shall follow HTTP status code conventions.

### 10.2 Data Format Standards
**NFR-10.2.1:** Data export shall support standard formats (JSON, CSV, XML).

**NFR-10.2.2:** Date/time formats shall follow ISO 8601 standard.

**NFR-10.2.3:** Geographic coordinates shall use WGS84 standard.

### 10.3 Integration Standards
**NFR-10.3.1:** System shall support webhook integration for event notifications.

**NFR-10.3.2:** System shall support OAuth2 for third-party authentication.

**NFR-10.3.3:** System shall support SAML for enterprise authentication.

---

## 11. Performance Optimization Requirements

### 11.1 Caching Strategy
**NFR-11.1.1:** Application-level caching shall be implemented using Redis.

**NFR-11.1.2:** Database query results shall be cached with TTL of 5 minutes.

**NFR-11.1.3:** GeoIP data shall be cached with TTL of 1 day.

**NFR-11.1.4:** User session data shall be cached in-memory.

### 11.2 Database Optimization
**NFR-11.2.1:** Database indexes shall be created for frequently queried columns.

**NFR-11.2.2:** Query optimization shall be performed regularly.

**NFR-11.2.3:** Database statistics shall be updated regularly.

**NFR-11.2.4:** Slow query logs shall be monitored and analyzed.

### 11.3 Frontend Optimization
**NFR-11.3.1:** JavaScript and CSS shall be minified and bundled.

**NFR-11.3.2:** Images shall be optimized and served in multiple formats.

**NFR-11.3.3:** Lazy loading shall be implemented for images and components.

**NFR-11.3.4:** Content delivery network (CDN) shall be used for static assets.

---

## 12. Backup & Recovery Requirements

### 12.1 Backup Strategy
**NFR-12.1.1:** Full database backup shall be performed daily.

**NFR-12.1.2:** Incremental backups shall be performed every 6 hours.

**NFR-12.1.3:** Backups shall be stored in geographically distributed locations.

**NFR-12.1.4:** Backup retention shall be at least 30 days.

### 12.2 Recovery Procedures
**NFR-12.2.1:** Backup restoration shall be tested monthly.

**NFR-12.2.2:** Recovery procedures shall be documented and tested.

**NFR-12.2.3:** Recovery time shall not exceed RTO of 1 hour.

---

## 13. Support & Maintenance Requirements

### 13.1 Support
**NFR-13.1.1:** Support shall be available 24/7 for critical issues.

**NFR-13.1.2:** Support response time shall be less than 2 hours.

**NFR-13.1.3:** Support resolution time shall be less than 24 hours for critical issues.

**NFR-13.1.4:** Support tickets shall be tracked and managed.

### 13.2 Maintenance
**NFR-13.2.1:** System updates shall be applied within 30 days of release.

**NFR-13.2.2:** Security patches shall be applied within 24 hours of release.

**NFR-13.2.3:** Maintenance shall be performed during scheduled maintenance windows.

---

## End of Non-Functional Requirements Document
