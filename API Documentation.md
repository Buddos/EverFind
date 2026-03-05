# API Documentation
## Hardware-Based Persistent Tracking System

**Document Version:** 1.0  
**Date:** March 2026  
**Status:** Final

**Base URL:** `https://api.devicetracking.com/v1`  
**Authentication:** JWT Bearer Token  
**Content-Type:** `application/json`

---

## 1. Authentication APIs

### 1.1 User Registration

**Endpoint:** `POST /auth/register`

**Description:** Register a new user account

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!",
  "full_name": "John Doe",
  "phone": "+254712345678",
  "address": "123 Main St, Nairobi"
}
```

**Response (201 Created):**
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "email": "user@example.com",
  "full_name": "John Doe",
  "phone": "+254712345678",
  "is_verified": false,
  "created_at": "2026-03-04T10:30:00Z"
}
```

**Error Response (400 Bad Request):**
```json
{
  "error": "Email already registered",
  "code": "EMAIL_EXISTS"
}
```

---

### 1.2 User Login

**Endpoint:** `POST /auth/login`

**Description:** Authenticate user and receive JWT token

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!"
}
```

**Response (200 OK):**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "full_name": "John Doe"
  }
}
```

---

### 1.3 Enable Two-Factor Authentication

**Endpoint:** `POST /auth/2fa/enable`

**Description:** Enable 2FA for user account

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "method": "sms"
}
```

**Response (200 OK):**
```json
{
  "secret": "JBSWY3DPEBLW64TMMQ======",
  "backup_codes": [
    "123456-789012",
    "345678-901234"
  ],
  "qr_code_url": "https://..."
}
```

---

## 2. Device Management APIs

### 2.1 Register Device

**Endpoint:** `POST /devices/register`

**Description:** Register a new device

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "device_type": "laptop",
  "manufacturer": "Dell",
  "model": "XPS 15",
  "serial_number": "ABC123XYZ",
  "color_description": "Silver",
  "purchase_date": "2023-06-15"
}
```

**Response (201 Created):**
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440001",
  "device_type": "laptop",
  "manufacturer": "Dell",
  "model": "XPS 15",
  "serial_number": "ABC123XYZ",
  "status": "active",
  "registration_date": "2026-03-04T10:30:00Z"
}
```

---

### 2.2 Get Device Details

**Endpoint:** `GET /devices/{device_id}`

**Description:** Retrieve device information

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (200 OK):**
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440001",
  "device_type": "laptop",
  "manufacturer": "Dell",
  "model": "XPS 15",
  "serial_number": "ABC123XYZ",
  "color_description": "Silver",
  "status": "active",
  "registration_date": "2026-03-04T10:30:00Z",
  "fingerprint": {
    "mac_ethernet": "00:1A:2B:3C:4D:5E",
    "mac_wifi": "00:1A:2B:3C:4D:5F",
    "confidence_score": 0.95
  }
}
```

---

### 2.3 List User Devices

**Endpoint:** `GET /devices`

**Description:** List all devices registered by user

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Parameters:**
- `status` (optional): Filter by status (active, stolen, recovered)
- `limit` (optional): Number of results (default: 10)
- `offset` (optional): Pagination offset (default: 0)

**Response (200 OK):**
```json
{
  "devices": [
    {
      "id": "660e8400-e29b-41d4-a716-446655440001",
      "device_type": "laptop",
      "manufacturer": "Dell",
      "model": "XPS 15",
      "status": "active"
    },
    {
      "id": "660e8400-e29b-41d4-a716-446655440002",
      "device_type": "smartphone",
      "manufacturer": "Apple",
      "model": "iPhone 14",
      "status": "stolen"
    }
  ],
  "total": 2,
  "limit": 10,
  "offset": 0
}
```

---

### 2.4 Update Device

**Endpoint:** `PUT /devices/{device_id}`

**Description:** Update device information

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "color_description": "Space Gray",
  "notes": "Updated description"
}
```

**Response (200 OK):**
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440001",
  "device_type": "laptop",
  "manufacturer": "Dell",
  "model": "XPS 15",
  "color_description": "Space Gray",
  "status": "active",
  "updated_at": "2026-03-04T11:00:00Z"
}
```

---

### 2.5 Delete Device

**Endpoint:** `DELETE /devices/{device_id}`

**Description:** Deregister a device

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (204 No Content)**

---

## 3. Hardware Fingerprinting APIs

### 3.1 Submit Fingerprint

**Endpoint:** `POST /fingerprints`

**Description:** Submit hardware fingerprint data

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "mac_ethernet": "00:1A:2B:3C:4D:5E",
  "mac_wifi": "00:1A:2B:3C:4D:5F",
  "mac_bluetooth": "00:1A:2B:3C:4D:60",
  "motherboard_serial": "CN1296034R00",
  "motherboard_manufacturer": "Dell",
  "bios_uuid": "4C4C4544-0050-4910-8037",
  "cpu_id": "BFEBFBFF000906E9",
  "cpu_model": "Intel i7-10750H",
  "storage_serial": "WD-WCC123456",
  "storage_model": "Samsung EVO 860",
  "storage_size_gb": 512
}
```

**Response (201 Created):**
```json
{
  "id": "770e8400-e29b-41d4-a716-446655440002",
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "confidence_score": 0.95,
  "captured_at": "2026-03-04T10:30:00Z",
  "status": "verified"
}
```

---

### 3.2 Get Fingerprint

**Endpoint:** `GET /fingerprints/{fingerprint_id}`

**Description:** Retrieve fingerprint data

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (200 OK):**
```json
{
  "id": "770e8400-e29b-41d4-a716-446655440002",
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "mac_ethernet": "00:1A:2B:3C:4D:5E",
  "mac_wifi": "00:1A:2B:3C:4D:5F",
  "confidence_score": 0.95,
  "captured_at": "2026-03-04T10:30:00Z"
}
```

---

## 4. Theft Reporting APIs

### 4.1 Report Device Theft

**Endpoint:** `POST /theft-reports`

**Description:** Report a device as stolen

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "theft_date": "2026-03-04T08:00:00Z",
  "theft_location": "Nairobi, Kenya",
  "circumstances": "Stolen from office",
  "police_report_number": "NRB/2026/001234"
}
```

**Response (201 Created):**
```json
{
  "id": "880e8400-e29b-41d4-a716-446655440003",
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "status": "active",
  "reported_at": "2026-03-04T10:30:00Z",
  "monitoring_activated": true
}
```

---

### 4.2 Get Theft Report

**Endpoint:** `GET /theft-reports/{report_id}`

**Description:** Retrieve theft report details

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (200 OK):**
```json
{
  "id": "880e8400-e29b-41d4-a716-446655440003",
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "theft_date": "2026-03-04T08:00:00Z",
  "theft_location": "Nairobi, Kenya",
  "circumstances": "Stolen from office",
  "police_report_number": "NRB/2026/001234",
  "status": "active",
  "reported_at": "2026-03-04T10:30:00Z"
}
```

---

### 4.3 Mark Device as Recovered

**Endpoint:** `PUT /theft-reports/{report_id}/recover`

**Description:** Mark a stolen device as recovered

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "recovery_date": "2026-03-05T14:00:00Z",
  "recovery_location": "Police Station, Nairobi"
}
```

**Response (200 OK):**
```json
{
  "id": "880e8400-e29b-41d4-a716-446655440003",
  "status": "recovered",
  "recovery_date": "2026-03-05T14:00:00Z",
  "recovery_location": "Police Station, Nairobi"
}
```

---

## 5. Location Tracking APIs

### 5.1 Get Location History

**Endpoint:** `GET /devices/{device_id}/locations`

**Description:** Get location history for a device

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Parameters:**
- `start_date` (optional): Start date (ISO 8601)
- `end_date` (optional): End date (ISO 8601)
- `limit` (optional): Number of results (default: 50)

**Response (200 OK):**
```json
{
  "locations": [
    {
      "id": "990e8400-e29b-41d4-a716-446655440004",
      "ip_address": "196.201.45.123",
      "latitude": -1.2921,
      "longitude": 36.8219,
      "city": "Nairobi",
      "country": "Kenya",
      "accuracy_meters": 500,
      "isp_name": "Safaricom",
      "detected_at": "2026-03-04T12:30:00Z"
    },
    {
      "id": "990e8400-e29b-41d4-a716-446655440005",
      "ip_address": "196.201.45.124",
      "latitude": -1.2950,
      "longitude": 36.8250,
      "city": "Nairobi",
      "country": "Kenya",
      "accuracy_meters": 500,
      "isp_name": "Safaricom",
      "detected_at": "2026-03-04T13:00:00Z"
    }
  ],
  "total": 2
}
```

---

### 5.2 Get Current Location

**Endpoint:** `GET /devices/{device_id}/current-location`

**Description:** Get most recent location of device

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (200 OK):**
```json
{
  "id": "990e8400-e29b-41d4-a716-446655440005",
  "ip_address": "196.201.45.124",
  "latitude": -1.2950,
  "longitude": 36.8250,
  "city": "Nairobi",
  "country": "Kenya",
  "accuracy_meters": 500,
  "isp_name": "Safaricom",
  "detected_at": "2026-03-04T13:00:00Z"
}
```

---

## 6. Evidence Generation APIs

### 6.1 Generate Evidence Report

**Endpoint:** `POST /evidence-reports`

**Description:** Generate evidence report for a theft

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "theft_report_id": "880e8400-e29b-41d4-a716-446655440003"
}
```

**Response (201 Created):**
```json
{
  "id": "aa0e8400-e29b-41d4-a716-446655440006",
  "theft_report_id": "880e8400-e29b-41d4-a716-446655440003",
  "report_status": "generating",
  "generated_at": "2026-03-04T14:00:00Z"
}
```

---

### 6.2 Get Evidence Report

**Endpoint:** `GET /evidence-reports/{report_id}`

**Description:** Retrieve generated evidence report

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (200 OK):**
```json
{
  "id": "aa0e8400-e29b-41d4-a716-446655440006",
  "theft_report_id": "880e8400-e29b-41d4-a716-446655440003",
  "evidence_summary": "Device detected online at coordinates...",
  "report_status": "completed",
  "blockchain_hash": "0x1234567890abcdef",
  "blockchain_tx_id": "0xabcdef1234567890",
  "generated_at": "2026-03-04T14:00:00Z",
  "download_url": "https://storage.example.com/evidence/aa0e8400.pdf"
}
```

---

### 6.3 Download Evidence Report

**Endpoint:** `GET /evidence-reports/{report_id}/download`

**Description:** Download evidence report as PDF

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response:** PDF file (application/pdf)

---

## 7. Police Portal APIs

### 7.1 Police Login

**Endpoint:** `POST /police/auth/login`

**Description:** Authenticate police user

**Request Body:**
```json
{
  "badge_number": "NRB-12345",
  "password": "SecurePassword123!"
}
```

**Response (200 OK):**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "bb0e8400-e29b-41d4-a716-446655440007",
    "full_name": "Detective John Smith",
    "badge_number": "NRB-12345",
    "department": "Nairobi Police",
    "role": "detective"
  }
}
```

---

### 7.2 Search Stolen Device

**Endpoint:** `GET /police/stolen-devices/search`

**Description:** Search for stolen devices

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Parameters:**
- `query` (required): Search term (device ID, MAC, serial, owner name)
- `limit` (optional): Number of results (default: 10)

**Response (200 OK):**
```json
{
  "results": [
    {
      "id": "660e8400-e29b-41d4-a716-446655440001",
      "device_type": "laptop",
      "manufacturer": "Dell",
      "model": "XPS 15",
      "owner": "John Doe",
      "theft_date": "2026-03-04T08:00:00Z",
      "current_location": {
        "city": "Nairobi",
        "country": "Kenya",
        "detected_at": "2026-03-04T13:00:00Z"
      }
    }
  ],
  "total": 1
}
```

---

### 7.3 Create Investigation

**Endpoint:** `POST /police/investigations`

**Description:** Create new investigation case

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "case_number": "NRB/2026/001234",
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "description": "Laptop stolen from office building"
}
```

**Response (201 Created):**
```json
{
  "id": "cc0e8400-e29b-41d4-a716-446655440008",
  "case_number": "NRB/2026/001234",
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "investigation_status": "open",
  "created_at": "2026-03-04T14:00:00Z"
}
```

---

### 7.4 Request Warrant

**Endpoint:** `POST /police/warrants`

**Description:** Request warrant for ISP data access

**Headers:**
```
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "investigation_id": "cc0e8400-e29b-41d4-a716-446655440008",
  "isp_partner_id": "dd0e8400-e29b-41d4-a716-446655440009",
  "warrant_details": "Court order to provide subscriber information for IP 196.201.45.124"
}
```

**Response (201 Created):**
```json
{
  "id": "ee0e8400-e29b-41d4-a716-446655440010",
  "investigation_id": "cc0e8400-e29b-41d4-a716-446655440008",
  "warrant_status": "pending",
  "requested_at": "2026-03-04T14:00:00Z"
}
```

---

## 8. Notification APIs

### 8.1 Get User Notifications

**Endpoint:** `GET /notifications`

**Description:** Get user notifications

**Headers:**
```
Authorization: Bearer {access_token}
```

**Query Parameters:**
- `is_read` (optional): Filter by read status (true/false)
- `limit` (optional): Number of results (default: 20)

**Response (200 OK):**
```json
{
  "notifications": [
    {
      "id": "ff0e8400-e29b-41d4-a716-446655440011",
      "notification_type": "device_detected",
      "title": "Device Detected Online",
      "message": "Your stolen device was detected online in Nairobi",
      "is_read": false,
      "created_at": "2026-03-04T13:00:00Z"
    }
  ],
  "total": 1
}
```

---

### 8.2 Mark Notification as Read

**Endpoint:** `PUT /notifications/{notification_id}/read`

**Description:** Mark notification as read

**Headers:**
```
Authorization: Bearer {access_token}
```

**Response (200 OK):**
```json
{
  "id": "ff0e8400-e29b-41d4-a716-446655440011",
  "is_read": true,
  "read_at": "2026-03-04T14:00:00Z"
}
```

---

## 9. Error Handling

### 9.1 Error Response Format

All error responses follow this format:

```json
{
  "error": "Error message",
  "code": "ERROR_CODE",
  "details": {
    "field": "Additional details"
  }
}
```

### 9.2 HTTP Status Codes

| Status Code | Meaning |
|-------------|---------|
| 200 | OK - Request successful |
| 201 | Created - Resource created |
| 204 | No Content - Successful deletion |
| 400 | Bad Request - Invalid input |
| 401 | Unauthorized - Authentication required |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource not found |
| 409 | Conflict - Resource already exists |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

---

## 10. Rate Limiting

**Rate Limits:**
- Public endpoints: 100 requests per minute per IP
- Authenticated endpoints: 1000 requests per minute per user
- Police endpoints: 500 requests per minute per user

**Headers:**
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1646395200
```

---

## 11. Pagination

All list endpoints support pagination using `limit` and `offset` parameters:

```
GET /devices?limit=10&offset=20
```

Response includes pagination metadata:

```json
{
  "data": [...],
  "total": 100,
  "limit": 10,
  "offset": 20
}
```

---

## 12. WebSocket API

### 12.1 Real-Time Location Updates

**Endpoint:** `wss://api.devicetracking.com/v1/ws/locations`

**Authentication:** JWT token in query parameter

**Connection:**
```
wss://api.devicetracking.com/v1/ws/locations?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Message Format:**
```json
{
  "type": "location_update",
  "device_id": "660e8400-e29b-41d4-a716-446655440001",
  "location": {
    "latitude": -1.2950,
    "longitude": 36.8250,
    "city": "Nairobi",
    "detected_at": "2026-03-04T13:00:00Z"
  }
}
```

---

## End of API Documentation
