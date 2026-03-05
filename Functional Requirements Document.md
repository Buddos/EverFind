# Functional Requirements Document
## Hardware-Based Persistent Tracking System

**Document Version:** 1.0  
**Date:** March 2026  
**Status:** Final

---

## 1. Introduction

### 1.1 Purpose
This document specifies the functional requirements for a comprehensive device tracking system that uses permanent hardware identifiers combined with ISP partnership to track and recover stolen laptops and gadgets even after complete OS reinstallation.

### 1.2 Scope
The system encompasses user registration, device fingerprinting, ISP integration, law enforcement collaboration, and evidence generation for legal proceedings.

### 1.3 Document Organization
- User Registration & Authentication Module
- Device Registration & Management Module
- Hardware Fingerprinting Module
- ISP Integration Module
- Stolen Device Management Module
- Evidence Generation & Reporting Module
- Police Portal Module
- Notification System Module

---

## 2. User Registration & Authentication Module

### 2.1 User Account Creation
**FR-2.1.1:** The system shall allow users to create an account using email and password.

**FR-2.1.2:** The system shall validate email format and enforce password complexity requirements (minimum 8 characters, including uppercase, lowercase, numbers, and special characters).

**FR-2.1.3:** The system shall send a verification email to the user's registered email address upon account creation.

**FR-2.1.4:** The system shall require users to verify their email address before accessing core features.

**FR-2.1.5:** The system shall support phone number registration and SMS-based OTP verification.

**FR-2.1.6:** The system shall allow optional social login integration with Google and Facebook.

### 2.2 Two-Factor Authentication (2FA)
**FR-2.2.1:** The system shall provide users the option to enable two-factor authentication.

**FR-2.2.2:** The system shall support SMS-based OTP as the primary 2FA method.

**FR-2.2.3:** The system shall support authenticator app-based 2FA (TOTP) as an alternative method.

**FR-2.2.4:** The system shall generate and display backup codes for account recovery when 2FA is enabled.

### 2.3 User Profile Management
**FR-2.3.1:** The system shall allow users to view and edit their profile information (name, email, phone, address).

**FR-2.3.2:** The system shall allow users to update their nearest police station information.

**FR-2.3.3:** The system shall allow users to add optional government ID information for police reports.

**FR-2.3.4:** The system shall allow users to change their password with current password verification.

**FR-2.3.5:** The system shall maintain an audit log of all profile changes with timestamps.

### 2.4 User Authentication
**FR-2.4.1:** The system shall authenticate users using email/password combination.

**FR-2.4.2:** The system shall implement session management with configurable timeout (default 30 minutes).

**FR-2.4.3:** The system shall support "Remember Me" functionality for 30 days.

**FR-2.4.4:** The system shall implement JWT-based token authentication for API access.

**FR-2.4.5:** The system shall support OAuth2 for third-party integrations.

---

## 3. Device Registration & Management Module

### 3.1 Device Registration
**FR-3.1.1:** The system shall provide three device registration methods:
- Manual entry via web form
- Automatic detection via desktop agent
- QR code scanning via mobile app

**FR-3.1.2:** The system shall require users to specify device type (Laptop, Smartphone, Tablet, Desktop).

**FR-3.1.3:** The system shall capture and store device manufacturer and model information.

**FR-3.1.4:** The system shall generate a unique device ID (UUID) upon successful registration.

**FR-3.1.5:** The system shall send confirmation email/SMS upon device registration.

**FR-3.1.6:** The system shall allow users to register multiple devices under a single account.

### 3.2 Device Information Management
**FR-3.2.1:** The system shall store the following device information:
- Device ID (UUID)
- Device type
- Manufacturer
- Model
- Serial number
- Purchase date
- Device color/description
- Registration date

**FR-3.2.2:** The system shall allow users to edit device information after registration.

**FR-3.2.3:** The system shall allow users to add photos/images of their registered devices.

**FR-3.2.4:** The system shall maintain version history of device information changes.

### 3.3 Device Status Management
**FR-3.3.1:** The system shall support the following device statuses:
- Active (registered and monitored)
- Inactive (temporarily disabled)
- Lost/Stolen (reported as stolen)
- Recovered (found and recovered)
- Deactivated (permanently removed from monitoring)

**FR-3.3.2:** The system shall allow users to change device status from the dashboard.

**FR-3.3.3:** The system shall timestamp all status changes for audit purposes.

### 3.4 Device Deregistration
**FR-3.4.1:** The system shall allow users to deregister a device.

**FR-3.4.2:** The system shall require confirmation before deregistration.

**FR-3.4.3:** The system shall archive deregistered device data for compliance purposes.

---

## 4. Hardware Fingerprinting Module

### 4.1 Desktop Agent Fingerprinting
**FR-4.1.1:** The system shall provide downloadable desktop agents for Windows, macOS, and Linux.

**FR-4.1.2:** The desktop agent shall capture the following hardware identifiers:
- MAC addresses (Ethernet, WiFi, Bluetooth)
- Motherboard serial number
- Motherboard manufacturer and version
- BIOS/UEFI UUID
- BIOS serial number and vendor
- Storage drive serial numbers and models
- CPU ID and model

**FR-4.1.3:** The desktop agent shall run once during device registration to capture fingerprints.

**FR-4.1.4:** The desktop agent shall securely transmit captured fingerprints to the backend server.

**FR-4.1.5:** The desktop agent shall display captured fingerprints to the user for verification before submission.

**FR-4.1.6:** The desktop agent shall be installable without administrative privileges (where possible).

### 4.2 Mobile Agent Fingerprinting
**FR-4.2.1:** The system shall provide mobile apps for iOS and Android.

**FR-4.2.2:** The mobile agent shall capture the following identifiers:
- WiFi MAC address
- Bluetooth MAC address
- Device ID (UDID for iOS, Android ID for Android)
- Device model and serial number
- IMEI (for phones)
- MEID (for CDMA phones)
- SIM card ICCID (if present)

**FR-4.2.3:** The mobile agent shall request necessary permissions from the user.

**FR-4.2.4:** The mobile agent shall securely transmit captured identifiers to the backend server.

### 4.3 Manual Fingerprint Entry
**FR-4.3.1:** The system shall provide a web form for manual entry of hardware identifiers.

**FR-4.3.2:** The manual entry form shall include validation for MAC address format.

**FR-4.3.3:** The system shall provide guidance on how to obtain hardware identifiers for different device types.

**FR-4.3.4:** The system shall validate entered information before storing.

### 4.4 Fingerprint Verification
**FR-4.4.1:** The system shall calculate a confidence score for each fingerprint based on the number and type of identifiers captured.

**FR-4.4.2:** The system shall flag fingerprints with low confidence scores for manual review.

**FR-4.4.3:** The system shall store fingerprints with encryption at rest.

---

## 5. ISP Integration Module

### 5.1 ISP Partner Management
**FR-5.1.1:** The system shall maintain a registry of ISP partners with the following information:
- ISP name
- API endpoint
- Authentication credentials
- Contact information
- Service level agreement (SLA)
- Coverage area

**FR-5.1.2:** The system shall support multiple ISP integrations simultaneously.

**FR-5.1.3:** The system shall implement ISP-specific API adapters for different providers.

### 5.2 Real-Time MAC-to-IP Lookup
**FR-5.2.1:** The system shall query ISP APIs to determine the current IP address assigned to a MAC address.

**FR-5.2.2:** The system shall implement caching to reduce API calls (cache TTL: 5 minutes).

**FR-5.2.3:** The system shall log all ISP API requests and responses for audit purposes.

**FR-5.2.4:** The system shall handle ISP API failures gracefully with fallback mechanisms.

**FR-5.2.5:** The system shall support batch MAC-to-IP lookups for efficiency.

### 5.3 IP Location Tracking
**FR-5.3.1:** The system shall use GeoIP databases to determine physical location from IP addresses.

**FR-5.3.2:** The system shall update GeoIP data regularly (at least monthly).

**FR-5.3.3:** The system shall store location history for each device with timestamps.

**FR-5.3.4:** The system shall calculate location confidence scores based on ISP data accuracy.

### 5.4 ISP Data Validation
**FR-5.4.1:** The system shall validate ISP-provided data for consistency and accuracy.

**FR-5.4.2:** The system shall cross-reference multiple ISP data sources when available.

**FR-5.4.3:** The system shall flag suspicious or inconsistent data for manual review.

---

## 6. Stolen Device Management Module

### 6.1 Device Theft Reporting
**FR-6.1.1:** The system shall allow users to report a device as stolen/lost.

**FR-6.1.2:** The theft report shall capture:
- Device ID
- Date and time of theft
- Location of theft
- Circumstances of theft
- Police report number (if filed)
- Insurance claim number (if applicable)

**FR-6.1.3:** The system shall send immediate notifications to the user upon theft report.

**FR-6.1.4:** The system shall activate enhanced monitoring for stolen devices.

**FR-6.1.5:** The system shall timestamp all theft reports for legal admissibility.

### 6.2 Stolen Device Database
**FR-6.2.1:** The system shall maintain a database of all reported stolen devices.

**FR-6.2.2:** The system shall allow police to query the stolen device database.

**FR-6.2.3:** The system shall provide real-time alerts when a stolen device connects to the network.

**FR-6.2.4:** The system shall track all access to stolen device information for audit purposes.

### 6.3 Device Recovery Tracking
**FR-6.3.1:** The system shall allow users to mark a device as recovered.

**FR-6.3.2:** The system shall capture recovery details (date, location, condition).

**FR-6.3.3:** The system shall generate recovery certificates for insurance claims.

**FR-6.3.4:** The system shall maintain recovery history for statistical analysis.

---

## 7. Evidence Generation & Reporting Module

### 7.1 Evidence Collection
**FR-7.1.1:** The system shall automatically collect evidence when a stolen device is detected online.

**FR-7.1.2:** Evidence shall include:
- Timestamp of detection
- MAC address detected
- IP address assigned
- ISP providing the data
- Geolocation information
- Device fingerprint match confidence

**FR-7.1.3:** The system shall store evidence with cryptographic signatures for integrity verification.

**FR-7.1.4:** The system shall maintain chain of custody documentation for all evidence.

### 7.2 Evidence Report Generation
**FR-7.2.1:** The system shall generate detailed evidence reports in PDF format.

**FR-7.2.2:** Evidence reports shall include:
- Device identification information
- Timeline of detections
- IP address history
- Geolocation data
- Fingerprint analysis
- Confidence scores
- Cryptographic verification

**FR-7.2.3:** The system shall allow users to download evidence reports.

**FR-7.2.4:** The system shall allow police to download evidence reports with additional details.

### 7.3 Blockchain Evidence Storage
**FR-7.3.1:** The system shall store evidence hashes on blockchain for immutability.

**FR-7.3.2:** The system shall provide blockchain transaction IDs for verification.

**FR-7.3.3:** The system shall support blockchain verification by third parties.

### 7.4 Legal Documentation
**FR-7.4.1:** The system shall generate warrant request templates for police use.

**FR-7.4.2:** The system shall provide affidavit templates for court proceedings.

**FR-7.4.3:** The system shall generate chain of custody documentation.

**FR-7.4.4:** The system shall provide evidence admissibility certification.

---

## 8. Police Portal Module

### 8.1 Police User Management
**FR-8.1.1:** The system shall support police department user accounts.

**FR-8.1.2:** Police users shall have role-based access control:
- Police Officer (view evidence, generate reports)
- Detective (full access to investigations)
- Police Station Admin (manage department users)

**FR-8.1.3:** The system shall require police badge number and department verification.

**FR-8.1.4:** The system shall maintain audit logs of all police user activities.

### 8.2 Device Search & Investigation
**FR-8.2.1:** The system shall allow police to search for stolen devices by:
- Device ID
- MAC address
- Device serial number
- Owner name
- Police report number

**FR-8.2.2:** The system shall display real-time location and IP address information for stolen devices.

**FR-8.2.3:** The system shall provide historical location data for investigation purposes.

**FR-8.2.4:** The system shall generate investigation timelines.

### 8.3 Warrant Management
**FR-8.3.1:** The system shall allow police to request warrants for ISP data access.

**FR-8.3.2:** The system shall generate warrant request templates with pre-filled information.

**FR-8.3.3:** The system shall track warrant status (pending, approved, denied, executed).

**FR-8.3.4:** The system shall integrate with ISP systems to execute approved warrants.

### 8.4 Case Management
**FR-8.4.1:** The system shall allow police to create and manage investigation cases.

**FR-8.4.2:** Each case shall link multiple stolen devices and evidence.

**FR-8.4.3:** The system shall support case status tracking (open, closed, recovered).

**FR-8.4.4:** The system shall allow case notes and annotations.

---

## 9. Notification System Module

### 9.1 User Notifications
**FR-9.1.1:** The system shall send notifications via:
- Email
- SMS
- In-app push notifications
- Dashboard alerts

**FR-9.1.2:** Notification triggers shall include:
- Device registration confirmation
- Stolen device detected online
- Location update
- Recovery confirmation
- Police investigation update

**FR-9.1.3:** The system shall allow users to configure notification preferences.

**FR-9.1.4:** The system shall support notification scheduling (quiet hours).

### 9.2 Police Notifications
**FR-9.2.1:** The system shall send immediate notifications to police when a stolen device is detected.

**FR-9.2.2:** Notifications shall include device details and current location.

**FR-9.2.3:** The system shall support escalation to higher authorities if needed.

### 9.3 ISP Notifications
**FR-9.3.1:** The system shall notify ISPs of warrant requests.

**FR-9.3.2:** The system shall track ISP response times.

---

## 10. Dashboard & Reporting Module

### 10.1 User Dashboard
**FR-10.1.1:** The system shall provide a user dashboard displaying:
- Registered devices and their status
- Stolen devices (if any)
- Recent activity
- Evidence reports
- Account settings

**FR-10.1.2:** The dashboard shall be responsive and mobile-friendly.

**FR-10.1.3:** The system shall provide quick actions for common tasks (register device, report theft, download evidence).

### 10.2 Admin Dashboard
**FR-10.2.1:** The system shall provide an admin dashboard for system administrators.

**FR-10.2.2:** Admin dashboard shall display:
- System statistics
- User metrics
- Device metrics
- ISP integration status
- System health
- Error logs

**FR-10.2.3:** The system shall provide user and device management tools for admins.

### 10.3 Analytics & Reporting
**FR-10.3.1:** The system shall generate reports on:
- Device registration trends
- Theft statistics
- Recovery rates
- Geographic hotspots
- ISP performance metrics

**FR-10.3.2:** The system shall support custom report generation.

**FR-10.3.3:** The system shall export reports in PDF and CSV formats.

---

## 11. API Module

### 11.1 REST API
**FR-11.1.1:** The system shall provide a comprehensive REST API for integrations.

**FR-11.1.2:** API endpoints shall be documented using OpenAPI/Swagger.

**FR-11.1.3:** The system shall support API rate limiting per user/application.

**FR-11.1.4:** The system shall require API key authentication.

### 11.2 WebSocket API
**FR-11.2.1:** The system shall provide WebSocket API for real-time updates.

**FR-11.2.2:** WebSocket connections shall support authentication and authorization.

**FR-11.2.3:** The system shall broadcast real-time events to connected clients.

---

## 12. Data Management Module

### 12.1 Data Export
**FR-12.1.1:** The system shall allow users to export their data in standard formats (JSON, CSV).

**FR-12.1.2:** The system shall support data portability as per GDPR requirements.

### 12.2 Data Deletion
**FR-12.2.1:** The system shall allow users to request data deletion (right to be forgotten).

**FR-12.2.2:** The system shall delete user data within 30 days of request.

**FR-12.2.3:** The system shall maintain evidence data as required by law.

---

## 13. Integration Module

### 13.1 Insurance Company Integration
**FR-13.1.1:** The system shall provide integration with insurance companies.

**FR-13.1.2:** The system shall allow insurance companies to query device status.

**FR-13.1.3:** The system shall provide evidence reports for insurance claims.

### 13.2 Third-Party Integrations
**FR-13.2.1:** The system shall support integration with device management platforms.

**FR-13.2.2:** The system shall provide webhooks for event notifications.

---

## End of Functional Requirements Document
