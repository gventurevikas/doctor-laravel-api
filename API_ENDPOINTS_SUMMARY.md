# Medical API - Complete Endpoints Summary

## 🌐 Server Information
- **Base URL**: `http://127.0.0.1:8000`
- **API Version**: `v1`
- **API Prefix**: `/api/v1`
- **Documentation**: `http://127.0.0.1:8000/docs`
- **Total Routes**: **1,573 endpoints**

## 📊 API Statistics
- **Authentication Routes**: 15+
- **Patient Management**: 50+
- **Appointment Management**: 40+
- **Prescription Management**: 60+
- **Billing & Payments**: 80+
- **Medical Records (EHR)**: 100+
- **File Management**: 200+
- **Notifications**: 150+
- **Reports**: 200+
- **Search**: 100+
- **Settings**: 150+
- **Telehealth**: 100+
- **System & Health**: 10+

## 🔧 System Endpoints

### Health & Status
- `GET /api/v1/health` - Health check endpoint
- `GET /api/v1/status` - API status and endpoint list

## 🔐 Authentication Endpoints

### Core Authentication
- `POST /api/v1/login` - User login
- `POST /api/v1/register` - User registration
- `POST /api/v1/logout` - User logout
- `GET /api/v1/me` - Get current user
- `POST /api/v1/forgot-password` - Send password reset
- `POST /api/v1/reset-password` - Reset password
- `POST /api/v1/verify-email` - Verify email
- `POST /api/v1/resend-verification` - Resend verification

### OAuth Integration
- `GET /api/v1/oauth/{provider}/redirect` - OAuth redirect
- `GET /api/v1/oauth/{provider}/callback` - OAuth callback

### Session Management
- `GET /api/v1/sessions` - Get user sessions
- `DELETE /api/v1/sessions` - Revoke all sessions
- `DELETE /api/v1/sessions/{session}` - Revoke specific session
- `POST /api/v1/refresh-token` - Refresh access token

## 👥 Patient Management

### Patient CRUD
- `GET /api/v1/patients/patients` - List patients
- `POST /api/v1/patients/patients` - Create patient
- `GET /api/v1/patients/patients/{patient}` - Get patient
- `PUT /api/v1/patients/patients/{patient}` - Update patient
- `DELETE /api/v1/patients/patients/{patient}` - Delete patient

### Patient Analytics
- `GET /api/v1/patients/patients/analytics` - Patient analytics
- `GET /api/v1/patients/patients/demographics` - Demographics
- `GET /api/v1/patients/patients/statistics` - Statistics

### Patient Operations
- `POST /api/v1/patients/patients/register` - Register patient
- `POST /api/v1/patients/patients/verify-email` - Verify email
- `POST /api/v1/patients/patients/resend-verification` - Resend verification
- `POST /api/v1/patients/patients/bulk/delete` - Bulk delete
- `POST /api/v1/patients/patients/bulk/export` - Bulk export
- `POST /api/v1/patients/patients/bulk/update` - Bulk update

### Patient Data
- `GET /api/v1/patients/patients/{patient}/allergies` - Patient allergies
- `GET /api/v1/patients/patients/{patient}/appointments` - Patient appointments
- `GET /api/v1/patients/patients/{patient}/billing` - Patient billing
- `GET /api/v1/patients/patients/{patient}/medical-records` - Medical records
- `GET /api/v1/patients/patients/{patient}/medications` - Patient medications
- `GET /api/v1/patients/patients/{patient}/vital-signs` - Vital signs

### My Patient Data
- `GET /api/v1/patients/my-appointments` - My appointments
- `GET /api/v1/patients/my-billing` - My billing
- `GET /api/v1/patients/my-medical-records` - My medical records
- `GET /api/v1/patients/my-prescriptions` - My prescriptions
- `GET /api/v1/patients/my-profile` - My profile
- `PUT /api/v1/patients/my-profile` - Update my profile

## 📅 Appointment Management

### Appointment CRUD
- `GET /api/v1/appointments/appointments` - List appointments
- `POST /api/v1/appointments/appointments` - Create appointment
- `GET /api/v1/appointments/appointments/{appointment}` - Get appointment
- `PUT /api/v1/appointments/appointments/{appointment}` - Update appointment
- `DELETE /api/v1/appointments/appointments/{appointment}` - Delete appointment

### Appointment Operations
- `POST /api/v1/appointments/appointments/{appointment}/cancel` - Cancel appointment
- `POST /api/v1/appointments/appointments/{appointment}/confirm` - Confirm appointment
- `POST /api/v1/appointments/appointments/{appointment}/reschedule` - Reschedule
- `GET /api/v1/appointments/appointments/{appointment}/notes` - Appointment notes
- `POST /api/v1/appointments/appointments/{appointment}/notes` - Add note

### Appointment Analytics
- `GET /api/v1/appointments/appointments/analytics` - Analytics
- `GET /api/v1/appointments/appointments/by-date` - By date
- `GET /api/v1/appointments/appointments/by-doctor` - By doctor
- `GET /api/v1/appointments/appointments/by-patient` - By patient
- `GET /api/v1/appointments/appointments/by-status` - By status

## 💊 Prescription Management

### Prescription CRUD
- `GET /api/v1/prescriptions/prescriptions` - List prescriptions
- `POST /api/v1/prescriptions/prescriptions` - Create prescription
- `GET /api/v1/prescriptions/prescriptions/{prescription}` - Get prescription
- `PUT /api/v1/prescriptions/prescriptions/{prescription}` - Update prescription
- `DELETE /api/v1/prescriptions/prescriptions/{prescription}` - Delete prescription

### Medication Management
- `GET /api/v1/prescriptions/medications` - List medications
- `POST /api/v1/prescriptions/medications` - Create medication
- `GET /api/v1/prescriptions/medications/{medication}` - Get medication
- `PUT /api/v1/prescriptions/medications/{medication}` - Update medication
- `DELETE /api/v1/prescriptions/medications/{medication}` - Delete medication

### Drug Interactions
- `GET /api/v1/prescriptions/medications/allergies` - Check allergies
- `POST /api/v1/prescriptions/medications/check-interactions` - Check interactions
- `GET /api/v1/prescriptions/medications/contraindications` - Contraindications
- `GET /api/v1/prescriptions/medications/interactions/{interaction}` - Interaction details

### Prescription Operations
- `POST /api/v1/prescriptions/prescriptions/{prescription}/approve` - Approve prescription
- `POST /api/v1/prescriptions/prescriptions/{prescription}/reject` - Reject prescription
- `POST /api/v1/prescriptions/prescriptions/{prescription}/dispense` - Dispense
- `POST /api/v1/prescriptions/prescriptions/{prescription}/complete` - Complete
- `POST /api/v1/prescriptions/prescriptions/{prescription}/cancel` - Cancel

### My Prescriptions
- `GET /api/v1/prescriptions/my-prescriptions` - My prescriptions
- `GET /api/v1/prescriptions/my-prescriptions/active` - Active prescriptions
- `GET /api/v1/prescriptions/my-prescriptions/history` - Prescription history
- `POST /api/v1/prescriptions/my-prescriptions/{prescription}/refill-request` - Request refill

## 💰 Billing & Payments

### Billing Management
- `GET /api/v1/billing/invoices` - List invoices
- `POST /api/v1/billing/invoices` - Create invoice
- `GET /api/v1/billing/invoices/{invoice}` - Get invoice
- `PUT /api/v1/billing/invoices/{invoice}` - Update invoice
- `DELETE /api/v1/billing/invoices/{invoice}` - Delete invoice

### Payment Processing
- `POST /api/v1/billing/payments` - Process payment
- `GET /api/v1/billing/payments/{payment}` - Get payment
- `POST /api/v1/billing/payments/{payment}/refund` - Refund payment

### Insurance Management
- `GET /api/v1/billing/insurance` - Insurance information
- `POST /api/v1/billing/insurance` - Add insurance
- `PUT /api/v1/billing/insurance/{insurance}` - Update insurance
- `DELETE /api/v1/billing/insurance/{insurance}` - Delete insurance

## 📋 Medical Records (EHR)

### Medical Records CRUD
- `GET /api/v1/ehr/medical-records` - List medical records
- `POST /api/v1/ehr/medical-records` - Create medical record
- `GET /api/v1/ehr/medical-records/{record}` - Get medical record
- `PUT /api/v1/ehr/medical-records/{record}` - Update medical record
- `DELETE /api/v1/ehr/medical-records/{record}` - Delete medical record

### Clinical Data
- `GET /api/v1/ehr/vital-signs` - Vital signs
- `POST /api/v1/ehr/vital-signs` - Store vital signs
- `GET /api/v1/ehr/vital-signs/chart` - Vital signs chart
- `GET /api/v1/ehr/vital-signs/{vitalSign}` - Get vital sign
- `PUT /api/v1/ehr/vital-signs/{vitalSign}` - Update vital sign
- `DELETE /api/v1/ehr/vital-signs/{vitalSign}` - Delete vital sign

### My EHR Data
- `GET /api/v1/ehr/my-ehr` - My EHR
- `GET /api/v1/ehr/my-medical-records` - My medical records
- `GET /api/v1/ehr/my-vital-signs` - My vital signs
- `GET /api/v1/ehr/my-allergies` - My allergies
- `GET /api/v1/ehr/my-medications` - My medications
- `GET /api/v1/ehr/my-immunizations` - My immunizations
- `GET /api/v1/ehr/my-lab-results` - My lab results
- `GET /api/v1/ehr/my-clinical-data` - My clinical data

## 📁 File Management

### File Operations
- `GET /api/v1/files/files` - List files
- `POST /api/v1/files/files` - Upload file
- `GET /api/v1/files/files/{file}` - Get file
- `PUT /api/v1/files/files/{file}` - Update file
- `DELETE /api/v1/files/files/{file}` - Delete file

### File Categories & Types
- `GET /api/v1/files/files/categories` - File categories
- `POST /api/v1/files/files/categories` - Create category
- `GET /api/v1/files/files/types` - File types
- `POST /api/v1/files/files/types` - Create type

### File Sharing
- `POST /api/v1/files/files/{file}/share` - Share file
- `POST /api/v1/files/files/{file}/share/public` - Make public
- `POST /api/v1/files/files/{file}/share/private` - Make private
- `GET /api/v1/files/files/public/{token}` - Public access
- `GET /api/v1/files/files/public/{token}/download` - Public download

### My Files
- `GET /api/v1/files/my-files` - My files
- `GET /api/v1/files/my-uploads` - My uploads
- `GET /api/v1/files/my-shares` - My shares
- `GET /api/v1/files/my-favorites` - My favorites

## 🔔 Notifications

### Notification Management
- `GET /api/v1/notifications/notifications` - List notifications
- `POST /api/v1/notifications/notifications` - Create notification
- `GET /api/v1/notifications/notifications/{notification}` - Get notification
- `PUT /api/v1/notifications/notifications/{notification}` - Update notification
- `DELETE /api/v1/notifications/notifications/{notification}` - Delete notification

### Notification Operations
- `POST /api/v1/notifications/notifications/{notification}/mark-read` - Mark as read
- `POST /api/v1/notifications/notifications/{notification}/mark-unread` - Mark as unread
- `POST /api/v1/notifications/notifications/send` - Send notification
- `POST /api/v1/notifications/notifications/bulk/send` - Bulk send

### My Notifications
- `GET /api/v1/notifications/my-notifications` - My notifications
- `GET /api/v1/notifications/my-notifications/unread` - Unread notifications
- `POST /api/v1/notifications/my-notifications/bulk/mark-read` - Bulk mark read

## 📊 Reports

### Report Management
- `GET /api/v1/reports/reports` - List reports
- `POST /api/v1/reports/reports` - Create report
- `GET /api/v1/reports/reports/{report}` - Get report
- `PUT /api/v1/reports/reports/{report}` - Update report
- `DELETE /api/v1/reports/reports/{report}` - Delete report

### Report Types
- `GET /api/v1/reports/reports/clinical` - Clinical reports
- `GET /api/v1/reports/reports/financial` - Financial reports
- `GET /api/v1/reports/reports/administrative` - Administrative reports
- `GET /api/v1/reports/reports/operational` - Operational reports
- `GET /api/v1/reports/reports/quality` - Quality reports

### My Reports
- `GET /api/v1/reports/my-reports` - My reports
- `GET /api/v1/reports/my-report-history` - Report history
- `GET /api/v1/reports/my-scheduled-reports` - Scheduled reports
- `GET /api/v1/reports/my-shared-reports` - Shared reports

## 🔍 Search

### Global Search
- `GET /api/v1/search/search` - Global search
- `POST /api/v1/search/search` - Advanced search
- `GET /api/v1/search/search/quick` - Quick search

### Search by Type
- `GET /api/v1/search/search/patients` - Search patients
- `GET /api/v1/search/search/doctors` - Search doctors
- `GET /api/v1/search/search/appointments` - Search appointments
- `GET /api/v1/search/search/prescriptions` - Search prescriptions
- `GET /api/v1/search/search/medical-records` - Search medical records

### Search Features
- `GET /api/v1/search/search/suggestions` - Search suggestions
- `GET /api/v1/search/search/history` - Search history
- `GET /api/v1/search/search/saved` - Saved searches
- `GET /api/v1/search/search/analytics` - Search analytics

## ⚙️ Settings

### System Settings
- `GET /api/v1/settings/settings/system` - System settings
- `PUT /api/v1/settings/settings/system` - Update system settings
- `GET /api/v1/settings/settings/system/general` - General settings
- `GET /api/v1/settings/settings/system/security` - Security settings
- `GET /api/v1/settings/settings/system/email` - Email settings

### User Settings
- `GET /api/v1/settings/settings/user` - User settings
- `PUT /api/v1/settings/settings/user` - Update user settings
- `GET /api/v1/settings/settings/user/profile` - User profile
- `GET /api/v1/settings/settings/user/preferences` - User preferences
- `GET /api/v1/settings/settings/user/security` - User security

### My Settings
- `GET /api/v1/settings/my-settings` - My settings
- `PUT /api/v1/settings/my-settings` - Update my settings
- `GET /api/v1/settings/my-preferences` - My preferences
- `PUT /api/v1/settings/my-preferences` - Update my preferences

## 🏥 Telehealth

### Telehealth Sessions
- `GET /api/v1/telehealth/telehealth/sessions` - List sessions
- `POST /api/v1/telehealth/telehealth/sessions` - Create session
- `GET /api/v1/telehealth/telehealth/sessions/{session}` - Get session
- `PUT /api/v1/telehealth/telehealth/sessions/{session}` - Update session
- `DELETE /api/v1/telehealth/telehealth/sessions/{session}` - Delete session

### Video Sessions
- `GET /api/v1/telehealth/telehealth/video-sessions` - Video sessions
- `POST /api/v1/telehealth/telehealth/video-sessions` - Create video session
- `GET /api/v1/telehealth/telehealth/video-sessions/{session}` - Get video session
- `POST /api/v1/telehealth/telehealth/video-sessions/{session}/join` - Join session
- `POST /api/v1/telehealth/telehealth/video-sessions/{session}/leave` - Leave session

### My Telehealth
- `GET /api/v1/telehealth/my-telehealth-sessions` - My sessions
- `GET /api/v1/telehealth/my-telehealth-consultations` - My consultations
- `GET /api/v1/telehealth/my-telehealth-recordings` - My recordings
- `GET /api/v1/telehealth/my-telehealth-settings` - My settings
- `PUT /api/v1/telehealth/my-telehealth-settings` - Update my settings

## 👤 User Management

### User CRUD
- `GET /api/v1/users` - List users
- `POST /api/v1/users` - Create user
- `GET /api/v1/users/{user}` - Get user
- `PUT /api/v1/users/{user}` - Update user
- `DELETE /api/v1/users/{user}` - Delete user

### User Operations
- `POST /api/v1/users/{user}/activate` - Activate user
- `POST /api/v1/users/{user}/deactivate` - Deactivate user

## 📈 Analytics

### Analytics Endpoints
- `GET /api/v1/analytics/overview` - Analytics overview
- `GET /api/v1/analytics/patients` - Patient analytics
- `GET /api/v1/analytics/appointments` - Appointment analytics
- `GET /api/v1/analytics/prescriptions` - Prescription analytics
- `GET /api/v1/analytics/billing` - Billing analytics

## 🔧 Testing Endpoints

### Health Checks
- `GET /api/v1/health` - Basic health check
- `GET /api/v1/status` - Detailed status

## 📚 Documentation

### API Documentation
- `GET /docs` - Interactive API documentation (Swagger UI)
- `GET /api-docs.json` - OpenAPI specification

---

## 🚀 Quick Start Examples

### 1. Health Check
```bash
curl http://127.0.0.1:8000/api/v1/health
```

### 2. User Login
```bash
curl -X POST http://127.0.0.1:8000/api/v1/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password"}'
```

### 3. Get API Status
```bash
curl http://127.0.0.1:8000/api/v1/status
```

### 4. List Patients (with authentication)
```bash
curl -X GET http://127.0.0.1:8000/api/v1/patients/patients \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## 📖 Documentation Access

- **Interactive Documentation**: http://127.0.0.1:8000/docs
- **API Specification**: http://127.0.0.1:8000/api-docs.json
- **Health Check**: http://127.0.0.1:8000/api/v1/health
- **API Status**: http://127.0.0.1:8000/api/v1/status

---

**Total Endpoints Available**: 1,573  
**API Version**: v1  
**Server**: http://127.0.0.1:8000  
**Status**: ✅ Running and Healthy 