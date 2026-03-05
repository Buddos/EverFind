# System Architecture & UML Diagrams
## Hardware-Based Persistent Tracking System

**Document Version:** 1.0  
**Date:** March 2026  
**Status:** Final

---

## 1. System Architecture Overview

### 1.1 High-Level Architecture

```mermaid
graph TB
    subgraph "User Layer"
        WEB["Web Portal<br/>(React)"]
        MOBILE["Mobile App<br/>(React Native)"]
        DESKTOP["Desktop Agent<br/>(Python/C#)"]
    end
    
    subgraph "API Gateway & Load Balancing"
        LB["Load Balancer<br/>(Nginx/HAProxy)"]
        APIGW["API Gateway<br/>(Kong/AWS API Gateway)"]
    end
    
    subgraph "Application Layer"
        AUTH["Authentication<br/>Service"]
        DEVICE["Device<br/>Management"]
        FINGERPRINT["Fingerprinting<br/>Engine"]
        TRACKING["Tracking<br/>Service"]
        EVIDENCE["Evidence<br/>Generation"]
        POLICE["Police<br/>Portal"]
        NOTIFICATION["Notification<br/>Service"]
    end
    
    subgraph "Integration Layer"
        ISP["ISP Integration<br/>Layer"]
        BLOCKCHAIN["Blockchain<br/>Integration"]
        GEOIP["GeoIP<br/>Service"]
    end
    
    subgraph "Data Layer"
        POSTGRES["PostgreSQL<br/>Database"]
        REDIS["Redis<br/>Cache"]
        S3["S3 Storage<br/>(Evidence)"]
    end
    
    subgraph "External Services"
        ISP_API["ISP APIs<br/>(Safaricom, Airtel)"]
        BLOCKCHAIN_NET["Blockchain<br/>Network"]
        EMAIL["Email Service<br/>(SendGrid)"]
        SMS["SMS Service<br/>(Twilio)"]
    end
    
    WEB --> LB
    MOBILE --> LB
    DESKTOP --> LB
    LB --> APIGW
    APIGW --> AUTH
    APIGW --> DEVICE
    APIGW --> FINGERPRINT
    APIGW --> TRACKING
    APIGW --> EVIDENCE
    APIGW --> POLICE
    APIGW --> NOTIFICATION
    
    AUTH --> POSTGRES
    DEVICE --> POSTGRES
    FINGERPRINT --> POSTGRES
    TRACKING --> POSTGRES
    EVIDENCE --> POSTGRES
    POLICE --> POSTGRES
    NOTIFICATION --> POSTGRES
    
    TRACKING --> REDIS
    DEVICE --> REDIS
    
    ISP --> ISP_API
    FINGERPRINT --> GEOIP
    EVIDENCE --> BLOCKCHAIN
    EVIDENCE --> S3
    
    BLOCKCHAIN --> BLOCKCHAIN_NET
    NOTIFICATION --> EMAIL
    NOTIFICATION --> SMS
```

---

## 2. Component Diagram

```mermaid
graph LR
    subgraph "Client Components"
        WEB_CLIENT["Web Client<br/>(React)"]
        MOBILE_CLIENT["Mobile Client<br/>(React Native)"]
        DESKTOP_CLIENT["Desktop Agent<br/>(Python)"]
    end
    
    subgraph "API Layer"
        REST_API["REST API<br/>(FastAPI/Express)"]
        WS_API["WebSocket API<br/>(Socket.io)"]
        GRAPHQL["GraphQL API<br/>(Optional)"]
    end
    
    subgraph "Business Logic"
        USER_SERVICE["User Service"]
        DEVICE_SERVICE["Device Service"]
        FINGERPRINT_SERVICE["Fingerprinting Service"]
        TRACKING_SERVICE["Tracking Service"]
        EVIDENCE_SERVICE["Evidence Service"]
        POLICE_SERVICE["Police Service"]
        NOTIFICATION_SERVICE["Notification Service"]
    end
    
    subgraph "Data Access"
        USER_DAO["User DAO"]
        DEVICE_DAO["Device DAO"]
        LOCATION_DAO["Location DAO"]
        EVIDENCE_DAO["Evidence DAO"]
    end
    
    subgraph "External Integrations"
        ISP_ADAPTER["ISP Adapter"]
        BLOCKCHAIN_ADAPTER["Blockchain Adapter"]
        GEOIP_ADAPTER["GeoIP Adapter"]
        EMAIL_ADAPTER["Email Adapter"]
        SMS_ADAPTER["SMS Adapter"]
    end
    
    subgraph "Infrastructure"
        DB["PostgreSQL"]
        CACHE["Redis"]
        QUEUE["Message Queue<br/>(RabbitMQ)"]
        STORAGE["S3 Storage"]
    end
    
    WEB_CLIENT --> REST_API
    MOBILE_CLIENT --> REST_API
    DESKTOP_CLIENT --> REST_API
    WEB_CLIENT --> WS_API
    MOBILE_CLIENT --> WS_API
    
    REST_API --> USER_SERVICE
    REST_API --> DEVICE_SERVICE
    REST_API --> TRACKING_SERVICE
    REST_API --> EVIDENCE_SERVICE
    REST_API --> POLICE_SERVICE
    
    USER_SERVICE --> USER_DAO
    DEVICE_SERVICE --> DEVICE_DAO
    TRACKING_SERVICE --> LOCATION_DAO
    EVIDENCE_SERVICE --> EVIDENCE_DAO
    
    USER_DAO --> DB
    DEVICE_DAO --> DB
    LOCATION_DAO --> DB
    EVIDENCE_DAO --> DB
    
    TRACKING_SERVICE --> CACHE
    DEVICE_SERVICE --> CACHE
    
    TRACKING_SERVICE --> ISP_ADAPTER
    EVIDENCE_SERVICE --> BLOCKCHAIN_ADAPTER
    TRACKING_SERVICE --> GEOIP_ADAPTER
    NOTIFICATION_SERVICE --> EMAIL_ADAPTER
    NOTIFICATION_SERVICE --> SMS_ADAPTER
    
    TRACKING_SERVICE --> QUEUE
    EVIDENCE_SERVICE --> QUEUE
    NOTIFICATION_SERVICE --> QUEUE
    
    EVIDENCE_SERVICE --> STORAGE
```

---

## 3. Use Case Diagram

```mermaid
graph TB
    subgraph "User Use Cases"
        UC1["Register Account"]
        UC2["Register Device"]
        UC3["View Dashboard"]
        UC4["Report Theft"]
        UC5["Download Evidence"]
        UC6["Manage Profile"]
    end
    
    subgraph "Police Use Cases"
        UC7["Search Stolen Device"]
        UC8["View Location History"]
        UC9["Generate Warrant"]
        UC10["Create Investigation"]
        UC11["View Evidence"]
    end
    
    subgraph "Admin Use Cases"
        UC12["Manage ISP Partners"]
        UC13["View System Metrics"]
        UC14["Manage Users"]
        UC15["Configure System"]
    end
    
    subgraph "System Use Cases"
        UC16["Detect Device Online"]
        UC17["Lookup IP Address"]
        UC18["Generate Evidence"]
        UC19["Send Notification"]
        UC20["Store Blockchain Record"]
    end
    
    USER["User"]
    POLICE["Police Officer"]
    ADMIN["System Admin"]
    SYSTEM["System"]
    
    USER --> UC1
    USER --> UC2
    USER --> UC3
    USER --> UC4
    USER --> UC5
    USER --> UC6
    
    POLICE --> UC7
    POLICE --> UC8
    POLICE --> UC9
    POLICE --> UC10
    POLICE --> UC11
    
    ADMIN --> UC12
    ADMIN --> UC13
    ADMIN --> UC14
    ADMIN --> UC15
    
    SYSTEM --> UC16
    SYSTEM --> UC17
    SYSTEM --> UC18
    SYSTEM --> UC19
    SYSTEM --> UC20
```

---

## 4. Activity Diagram - Device Registration Flow

```mermaid
graph TD
    START([User Initiates Registration])
    
    CHOICE1{Registration Method?}
    
    MANUAL["Manual Entry<br/>via Web Form"]
    AGENT["Download Desktop Agent"]
    MOBILE_REG["Mobile App<br/>Registration"]
    
    MANUAL --> ENTER["Enter Device Details"]
    AGENT --> DOWNLOAD["Download & Install Agent"]
    MOBILE_REG --> SCAN["Scan QR Code"]
    
    DOWNLOAD --> CAPTURE["Agent Captures<br/>Hardware Fingerprints"]
    ENTER --> VALIDATE1["Validate Input"]
    SCAN --> CAPTURE
    
    CAPTURE --> VERIFY["User Verifies<br/>Captured Data"]
    VALIDATE1 --> VERIFY
    
    VERIFY --> SUBMIT["Submit to Server"]
    
    SUBMIT --> VALIDATE2["Server Validates<br/>Fingerprints"]
    
    VALIDATE2 --> CALC_SCORE["Calculate<br/>Confidence Score"]
    
    CALC_SCORE --> CHECK_SCORE{Confidence<br/>Score OK?}
    
    CHECK_SCORE -->|Low| REVIEW["Manual Review<br/>by Admin"]
    CHECK_SCORE -->|High| GENERATE["Generate Device ID"]
    
    REVIEW --> APPROVE{Approved?}
    APPROVE -->|No| REJECT["Reject Registration"]
    APPROVE -->|Yes| GENERATE
    
    GENERATE --> STORE["Store in Database"]
    
    STORE --> NOTIFY["Send Confirmation<br/>Email/SMS"]
    
    NOTIFY --> END([Registration Complete])
    
    REJECT --> END_REJECT([Registration Failed])
    
    START --> CHOICE1
    CHOICE1 --> MANUAL
    CHOICE1 --> AGENT
    CHOICE1 --> MOBILE_REG
```

---

## 5. Activity Diagram - Theft Detection & Evidence Flow

```mermaid
graph TD
    START([Stolen Device Reported])
    
    ACTIVATE["Activate Enhanced<br/>Monitoring"]
    
    ACTIVATE --> WAIT["Wait for Device<br/>to Connect"]
    
    WAIT --> DETECT{Device<br/>Connected?}
    
    DETECT -->|No| WAIT
    DETECT -->|Yes| CAPTURE_MAC["Capture MAC Address"]
    
    CAPTURE_MAC --> ISP_LOOKUP["Query ISP for<br/>Current IP"]
    
    ISP_LOOKUP --> GET_IP["Retrieve IP Address"]
    
    GET_IP --> GEOIP["Perform GeoIP<br/>Lookup"]
    
    GEOIP --> GET_LOCATION["Get Location<br/>Coordinates"]
    
    GET_LOCATION --> STORE_LOCATION["Store Location<br/>in Database"]
    
    STORE_LOCATION --> GENERATE_EVIDENCE["Generate Evidence<br/>Report"]
    
    GENERATE_EVIDENCE --> SIGN["Digitally Sign<br/>Evidence"]
    
    SIGN --> BLOCKCHAIN["Store Hash on<br/>Blockchain"]
    
    BLOCKCHAIN --> NOTIFY_USER["Notify User of<br/>Detection"]
    
    NOTIFY_USER --> NOTIFY_POLICE["Notify Police<br/>Department"]
    
    NOTIFY_POLICE --> CREATE_CASE["Create Investigation<br/>Case"]
    
    CREATE_CASE --> END([Evidence Ready for<br/>Law Enforcement])
```

---

## 6. Activity Diagram - Police Investigation Workflow

```mermaid
graph TD
    START([Police Receives Alert])
    
    SEARCH["Search Stolen Device<br/>Database"]
    
    SEARCH --> FIND{Device<br/>Found?}
    
    FIND -->|No| END_NOT_FOUND([Case Closed])
    FIND -->|Yes| VIEW_EVIDENCE["View Evidence<br/>& Location History"]
    
    VIEW_EVIDENCE --> ANALYZE["Analyze Location<br/>Pattern"]
    
    ANALYZE --> DECISION{Warrant<br/>Needed?}
    
    DECISION -->|No| DISPATCH["Dispatch Police<br/>to Location"]
    DECISION -->|Yes| GENERATE_WARRANT["Generate Warrant<br/>Request"]
    
    GENERATE_WARRANT --> SUBMIT_WARRANT["Submit to Judge"]
    
    SUBMIT_WARRANT --> APPROVE{Warrant<br/>Approved?}
    
    APPROVE -->|No| END_DENIED([Case Closed])
    APPROVE -->|Yes| EXECUTE["Execute Warrant<br/>with ISP"]
    
    EXECUTE --> GET_DATA["Retrieve Subscriber<br/>Information"]
    
    GET_DATA --> DISPATCH
    
    DISPATCH --> RECOVERY{Device<br/>Recovered?}
    
    RECOVERY -->|Yes| CLOSE_SUCCESS["Close Case -<br/>Recovered"]
    RECOVERY -->|No| CONTINUE["Continue<br/>Investigation"]
    
    CONTINUE --> WAIT["Wait for Next<br/>Detection"]
    WAIT --> RECOVERY
    
    CLOSE_SUCCESS --> END([Case Closed -<br/>Device Recovered])
```

---

## 7. Sequence Diagram - Device Registration

```mermaid
sequenceDiagram
    participant User
    participant WebPortal
    participant Backend
    participant Database
    participant EmailService
    
    User->>WebPortal: Click "Register Device"
    WebPortal->>User: Show registration form
    User->>WebPortal: Enter device details
    WebPortal->>Backend: POST /api/devices/register
    Backend->>Backend: Validate input
    Backend->>Database: Store device record
    Database-->>Backend: Device ID generated
    Backend->>Backend: Generate fingerprint request
    Backend-->>WebPortal: Return device ID & fingerprint form
    WebPortal->>User: Show fingerprint form
    User->>WebPortal: Enter/capture fingerprints
    WebPortal->>Backend: POST /api/fingerprints
    Backend->>Backend: Validate fingerprints
    Backend->>Backend: Calculate confidence score
    Backend->>Database: Store fingerprints
    Database-->>Backend: Confirmation
    Backend->>EmailService: Send confirmation email
    EmailService-->>User: Confirmation email received
    Backend-->>WebPortal: Registration complete
    WebPortal->>User: Show success message
```

---

## 8. Sequence Diagram - Theft Detection & Tracking

```mermaid
sequenceDiagram
    participant User
    participant System
    participant ISP
    participant GeoIP
    participant Police
    participant Blockchain
    
    User->>System: Report device stolen
    System->>System: Activate monitoring
    System->>System: Wait for connection
    System->>ISP: Query MAC address
    ISP-->>System: Return current IP
    System->>GeoIP: Lookup IP location
    GeoIP-->>System: Return coordinates
    System->>System: Generate evidence report
    System->>Blockchain: Store evidence hash
    Blockchain-->>System: Transaction confirmed
    System->>Police: Send alert with location
    Police-->>System: Acknowledge receipt
    System->>User: Send notification
    User-->>System: Acknowledge
    Police->>System: Request warrant
    System->>ISP: Execute warrant
    ISP-->>Police: Provide subscriber info
    Police->>Police: Dispatch to location
    Police->>System: Report device recovered
    System->>User: Send recovery notification
```

---

## 9. Sequence Diagram - Evidence Generation

```mermaid
sequenceDiagram
    participant Police
    participant System
    participant Database
    participant Blockchain
    participant Storage
    participant Police2 as Police Officer
    
    Police->>System: Request evidence report
    System->>Database: Query location history
    Database-->>System: Return locations
    System->>Database: Query fingerprints
    Database-->>System: Return fingerprints
    System->>Database: Query ISP lookups
    Database-->>System: Return ISP data
    System->>System: Compile evidence
    System->>System: Generate PDF report
    System->>System: Calculate hash
    System->>Blockchain: Store evidence hash
    Blockchain-->>System: Transaction ID
    System->>Storage: Upload PDF to S3
    Storage-->>System: File URL
    System->>Database: Store evidence record
    System-->>Police: Return evidence report
    Police->>Storage: Download PDF
    Storage-->>Police2: PDF file
    Police2->>Police2: Review evidence
    Police2->>System: Submit to court
```

---

## 10. State Diagram - Device Status Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Registered: Device Registered
    
    Registered --> Active: Activation
    Registered --> Inactive: Deactivation
    
    Active --> Lost: Theft Reported
    Active --> Inactive: User Disables
    Active --> Deregistered: User Removes
    
    Inactive --> Active: User Reactivates
    Inactive --> Deregistered: User Removes
    
    Lost --> Recovered: Device Found
    Lost --> Abandoned: No Activity (30 days)
    
    Recovered --> Active: Device Returned
    Recovered --> Deregistered: User Removes
    
    Abandoned --> Deregistered: User Removes
    
    Deregistered --> [*]
```

---

## 11. Deployment Architecture

```mermaid
graph TB
    subgraph "CDN Layer"
        CDN["CloudFlare CDN<br/>(Static Assets)"]
    end
    
    subgraph "Load Balancing"
        ALB["AWS ALB<br/>(Application Load Balancer)"]
    end
    
    subgraph "Kubernetes Cluster"
        subgraph "API Pods"
            API1["API Pod 1"]
            API2["API Pod 2"]
            API3["API Pod 3"]
        end
        
        subgraph "Worker Pods"
            WORKER1["Worker Pod 1"]
            WORKER2["Worker Pod 2"]
        end
        
        subgraph "Cache"
            REDIS_CLUSTER["Redis Cluster"]
        end
    end
    
    subgraph "Database Layer"
        PRIMARY["PostgreSQL Primary<br/>(us-east-1)"]
        REPLICA1["PostgreSQL Replica<br/>(us-west-1)"]
        REPLICA2["PostgreSQL Replica<br/>(eu-west-1)"]
    end
    
    subgraph "Storage"
        S3["AWS S3<br/>(Evidence Storage)"]
        S3_BACKUP["S3 Backup<br/>(Cross-region)"]
    end
    
    subgraph "Monitoring"
        PROMETHEUS["Prometheus<br/>(Metrics)"]
        GRAFANA["Grafana<br/>(Dashboards)"]
        ELK["ELK Stack<br/>(Logs)"]
    end
    
    CDN --> ALB
    ALB --> API1
    ALB --> API2
    ALB --> API3
    
    API1 --> REDIS_CLUSTER
    API2 --> REDIS_CLUSTER
    API3 --> REDIS_CLUSTER
    
    API1 --> PRIMARY
    API2 --> PRIMARY
    API3 --> PRIMARY
    
    PRIMARY --> REPLICA1
    PRIMARY --> REPLICA2
    
    API1 --> S3
    API2 --> S3
    API3 --> S3
    
    S3 --> S3_BACKUP
    
    WORKER1 --> PRIMARY
    WORKER2 --> PRIMARY
    
    API1 --> PROMETHEUS
    API2 --> PROMETHEUS
    API3 --> PROMETHEUS
    
    PROMETHEUS --> GRAFANA
    
    API1 --> ELK
    API2 --> ELK
    API3 --> ELK
```

---

## 12. Data Flow Diagram - Level 0

```mermaid
graph LR
    USER["👤 User"]
    POLICE["👮 Police"]
    ISP["🌐 ISP"]
    
    SYSTEM["🔒 Device Tracking<br/>System"]
    
    USER -->|Register Device| SYSTEM
    USER -->|Report Theft| SYSTEM
    USER -->|Download Evidence| SYSTEM
    
    SYSTEM -->|Notify| USER
    
    POLICE -->|Search Device| SYSTEM
    POLICE -->|Request Warrant| SYSTEM
    POLICE -->|View Evidence| SYSTEM
    
    SYSTEM -->|Alert| POLICE
    
    ISP -->|Provide IP Data| SYSTEM
    SYSTEM -->|Query MAC-to-IP| ISP
```

---

## 13. Data Flow Diagram - Level 1

```mermaid
graph TB
    subgraph "User Layer"
        USER["User"]
        POLICE["Police"]
    end
    
    subgraph "System"
        AUTH["1. Authentication"]
        DEVICE_MGT["2. Device Management"]
        TRACKING["3. Tracking Engine"]
        EVIDENCE["4. Evidence Generation"]
    end
    
    subgraph "Data Store"
        DB["Database"]
        CACHE["Cache"]
    end
    
    subgraph "External"
        ISP["ISP APIs"]
        BLOCKCHAIN["Blockchain"]
    end
    
    USER -->|Login| AUTH
    AUTH -->|Validate| DB
    AUTH -->|Cache Session| CACHE
    
    USER -->|Register Device| DEVICE_MGT
    DEVICE_MGT -->|Store| DB
    DEVICE_MGT -->|Cache| CACHE
    
    USER -->|Report Theft| TRACKING
    TRACKING -->|Query ISP| ISP
    TRACKING -->|Store Location| DB
    TRACKING -->|Cache| CACHE
    
    TRACKING -->|Generate Evidence| EVIDENCE
    EVIDENCE -->|Store Evidence| DB
    EVIDENCE -->|Store Hash| BLOCKCHAIN
    
    POLICE -->|Search| TRACKING
    POLICE -->|View Evidence| EVIDENCE
```

---

## 14. Network Diagram

```mermaid
graph TB
    subgraph "Internet"
        USERS["Users<br/>(Global)"]
        POLICE["Police<br/>(Kenya)"]
        ISP_PARTNERS["ISP Partners<br/>(Kenya)"]
    end
    
    subgraph "AWS Cloud"
        subgraph "VPC"
            subgraph "Public Subnet"
                ALB["Application<br/>Load Balancer"]
                NAT["NAT Gateway"]
            end
            
            subgraph "Private Subnet 1"
                K8S["Kubernetes Cluster<br/>(API Servers)"]
            end
            
            subgraph "Private Subnet 2"
                DB["PostgreSQL<br/>Database"]
                REDIS["Redis<br/>Cache"]
            end
            
            subgraph "Private Subnet 3"
                WORKERS["Background<br/>Workers"]
            end
        end
        
        subgraph "Storage"
            S3["S3 Buckets<br/>(Evidence)"]
        end
    end
    
    USERS -->|HTTPS| ALB
    POLICE -->|HTTPS| ALB
    ISP_PARTNERS -->|API| ALB
    
    ALB --> K8S
    K8S --> NAT
    
    K8S --> DB
    K8S --> REDIS
    K8S --> S3
    
    WORKERS --> DB
    WORKERS --> REDIS
    
    NAT --> USERS
    NAT --> ISP_PARTNERS
```

---

## End of System Architecture & UML Diagrams
