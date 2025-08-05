# 🔌 ProConnectHub - API Specifications

## 📋 Overview
Complete RESTful API specification for ProConnectHub client management and service platform.

## 🏗️ API Design Principles
- **RESTful Architecture**: Standard HTTP methods and status codes
- **Consistent Responses**: Uniform response structure across all endpoints
- **Authentication**: JWT-based authentication with role-based access
- **Versioning**: API versioning with `/api/v1/` prefix
- **Rate Limiting**: Request throttling to prevent abuse
- **Data Validation**: Comprehensive input validation

## 🔐 Authentication & Authorization

### JWT Token Structure
```json
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "userId": 123,
    "email": "user@example.com",
    "roles": ["professional"],
    "professionalId": 456,
    "iat": 1634567890,
    "exp": 1634654290
  }
}
```

### Authorization Header
```
Authorization: Bearer <jwt_token>
```

## 📊 Standard Response Format

### Success Response
```json
{
  "success": true,
  "data": {},
  "message": "Operation successful",
  "timestamp": "2024-01-01T12:00:00Z",
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

### Error Response
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ]
  },
  "timestamp": "2024-01-01T12:00:00Z"
}
```

## 🔑 Authentication Endpoints

### POST /api/v1/auth/register
Register a new user account.

**Request Body:**
```json
{
  "email": "user@example.com",
  "phone": "+91-9876543210",
  "password": "SecurePassword123",
  "firstName": "John",
  "lastName": "Doe",
  "role": "professional", // or "client"
  "professionalData": { // Only if role is "professional"
    "professionType": "doctor",
    "specialty": "General Medicine",
    "licenseNumber": "MED123456"
  }
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "userId": 123,
    "email": "user@example.com",
    "verificationRequired": true,
    "message": "Please verify your email to activate your account"
  }
}
```

### POST /api/v1/auth/verify-email
Verify email address with OTP.

**Request Body:**
```json
{
  "email": "user@example.com",
  "otp": "123456"
}
```

### POST /api/v1/auth/login
User login authentication.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "refresh_token_here",
    "user": {
      "id": 123,
      "email": "user@example.com",
      "firstName": "John",
      "lastName": "Doe",
      "roles": ["professional"],
      "profileComplete": true
    },
    "expiresIn": 3600
  }
}
```

### POST /api/v1/auth/refresh
Refresh access token.

**Request Body:**
```json
{
  "refreshToken": "refresh_token_here"
}
```

### POST /api/v1/auth/forgot-password
Request password reset.

**Request Body:**
```json
{
  "email": "user@example.com"
}
```

### POST /api/v1/auth/reset-password
Reset password with token.

**Request Body:**
```json
{
  "token": "reset_token",
  "newPassword": "NewSecurePassword123"
}
```

### POST /api/v1/auth/logout
Logout user and invalidate tokens.

## 👤 User Management Endpoints

### GET /api/v1/users/profile
Get current user profile.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": 123,
    "email": "user@example.com",
    "phone": "+91-9876543210",
    "firstName": "John",
    "lastName": "Doe",
    "profileImage": "https://example.com/images/profile.jpg",
    "emailVerified": true,
    "phoneVerified": true,
    "isActive": true,
    "lastLogin": "2024-01-01T12:00:00Z",
    "roles": ["professional"],
    "professionalProfile": {
      "id": 456,
      "professionType": "doctor",
      "specialty": "General Medicine",
      "licenseNumber": "MED123456",
      "isVerified": true
    }
  }
}
```

### PUT /api/v1/users/profile
Update user profile.

**Request Body:**
```json
{
  "firstName": "John",
  "lastName": "Smith",
  "phone": "+91-9876543210",
  "profileImage": "base64_image_data"
}
```

### PUT /api/v1/users/change-password
Change user password.

**Request Body:**
```json
{
  "currentPassword": "CurrentPassword123",
  "newPassword": "NewPassword123"
}
```

## 🏥 Professional Management Endpoints

### GET /api/v1/professionals/profile
Get professional profile details.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": 456,
    "userId": 123,
    "professionType": "doctor",
    "specialty": "General Medicine",
    "licenseNumber": "MED123456",
    "experienceYears": 10,
    "qualifications": "MBBS, MD",
    "bio": "Experienced general practitioner...",
    "consultationFee": 500.00,
    "currency": "INR",
    "timezone": "Asia/Kolkata",
    "address": {
      "line1": "123 Medical Street",
      "line2": "Near City Hospital",
      "city": "Mumbai",
      "state": "Maharashtra",
      "country": "India",
      "postalCode": "400001"
    },
    "businessInfo": {
      "name": "Dr. John's Clinic",
      "registration": "CLINIC123",
      "taxId": "TAX123456"
    },
    "isVerified": true,
    "verificationDocuments": ["license.pdf", "degree.pdf"]
  }
}
```

### PUT /api/v1/professionals/profile
Update professional profile.

### POST /api/v1/professionals/upload-documents
Upload verification documents.

**Request (Multipart Form):**
```
documents: [File, File, ...]
documentTypes: ["license", "degree", "certificate"]
```

### GET /api/v1/professionals/search
Search professionals.

**Query Parameters:**
- `profession`: Profession type filter
- `specialty`: Specialty filter
- `city`: Location filter
- `page`: Page number
- `limit`: Results per page

## 👥 Client Management Endpoints

### GET /api/v1/clients
Get all clients for the authenticated professional.

**Query Parameters:**
- `page`: Page number (default: 1)
- `limit`: Results per page (default: 20)
- `search`: Search by name or email
- `tags`: Filter by tags
- `isActive`: Filter by active status

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 789,
      "userId": 101,
      "user": {
        "firstName": "Jane",
        "lastName": "Doe",
        "email": "jane@example.com",
        "phone": "+91-9876543211",
        "profileImage": "profile.jpg"
      },
      "dateOfBirth": "1990-05-15",
      "gender": "female",
      "bloodGroup": "O+",
      "emergencyContact": {
        "name": "John Doe",
        "phone": "+91-9876543212"
      },
      "address": {
        "line1": "456 Client Street",
        "city": "Delhi",
        "state": "Delhi",
        "postalCode": "110001"
      },
      "tags": ["regular_patient", "diabetes"],
      "isActive": true,
      "lastAppointment": "2024-01-01T10:00:00Z",
      "totalAppointments": 5,
      "createdAt": "2023-06-01T12:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 45,
    "totalPages": 3
  }
}
```

### POST /api/v1/clients
Add a new client.

**Request Body:**
```json
{
  "email": "newclient@example.com",
  "firstName": "Alice",
  "lastName": "Johnson",
  "phone": "+91-9876543213",
  "dateOfBirth": "1985-03-20",
  "gender": "female",
  "bloodGroup": "A+",
  "emergencyContact": {
    "name": "Bob Johnson",
    "phone": "+91-9876543214"
  },
  "address": {
    "line1": "789 New Street",
    "city": "Bangalore",
    "state": "Karnataka",
    "postalCode": "560001"
  },
  "medicalHistory": "No significant medical history",
  "allergies": "None known",
  "currentMedications": "None",
  "tags": ["new_patient"],
  "notes": "Referred by Dr. Smith"
}
```

### GET /api/v1/clients/:id
Get specific client details.

### PUT /api/v1/clients/:id
Update client information.

### DELETE /api/v1/clients/:id
Deactivate a client.

### POST /api/v1/clients/:id/notes
Add notes for a client.

**Request Body:**
```json
{
  "note": "Patient showed improvement in symptoms",
  "type": "consultation_note",
  "appointmentId": 123
}
```

### GET /api/v1/clients/:id/notes
Get all notes for a client.

### GET /api/v1/clients/:id/history
Get client's appointment and medical history.

## 📅 Appointment Management Endpoints

### GET /api/v1/appointments
Get appointments for authenticated user.

**Query Parameters:**
- `date`: Specific date (YYYY-MM-DD)
- `startDate`: Date range start
- `endDate`: Date range end
- `status`: Filter by status
- `clientId`: Filter by client
- `page`: Page number
- `limit`: Results per page

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1001,
      "professionalId": 456,
      "clientId": 789,
      "client": {
        "firstName": "Jane",
        "lastName": "Doe",
        "phone": "+91-9876543211"
      },
      "serviceId": 111,
      "service": {
        "name": "General Consultation",
        "duration": 30,
        "price": 500.00
      },
      "appointmentDate": "2024-01-15",
      "startTime": "10:00:00",
      "endTime": "10:30:00",
      "appointmentType": "online",
      "status": "confirmed",
      "meetingLink": "https://meet.jit.si/appointment-1001",
      "notes": "Regular checkup",
      "reminderSent": true,
      "createdAt": "2024-01-01T12:00:00Z"
    }
  ]
}
```

### POST /api/v1/appointments
Book a new appointment.

**Request Body:**
```json
{
  "clientId": 789,
  "serviceId": 111,
  "appointmentDate": "2024-01-15",
  "startTime": "10:00:00",
  "appointmentType": "online",
  "notes": "Regular checkup"
}
```

### GET /api/v1/appointments/:id
Get specific appointment details.

### PUT /api/v1/appointments/:id
Update appointment.

### DELETE /api/v1/appointments/:id
Cancel appointment.

**Request Body:**
```json
{
  "cancellationReason": "Patient requested cancellation"
}
```

### POST /api/v1/appointments/:id/reschedule
Reschedule appointment.

**Request Body:**
```json
{
  "newDate": "2024-01-16",
  "newStartTime": "11:00:00",
  "reason": "Scheduling conflict"
}
```

### POST /api/v1/appointments/:id/complete
Mark appointment as completed.

**Request Body:**
```json
{
  "consultationNotes": "Patient is recovering well",
  "prescriptions": [
    {
      "medication": "Paracetamol",
      "dosage": "500mg",
      "frequency": "Twice daily",
      "duration": "5 days"
    }
  ],
  "followUpRequired": true,
  "followUpDate": "2024-01-22"
}
```

## 🛠️ Service Management Endpoints

### GET /api/v1/services
Get all services for authenticated professional.

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 111,
      "professionalId": 456,
      "name": "General Consultation",
      "description": "Comprehensive health checkup and consultation",
      "category": "consultation",
      "duration": 30,
      "price": 500.00,
      "currency": "INR",
      "pricingType": "fixed",
      "onlineAvailable": true,
      "offlineAvailable": true,
      "maxAdvanceBookingDays": 30,
      "bufferTime": 15,
      "isActive": true,
      "bookingCount": 150,
      "createdAt": "2023-01-01T12:00:00Z"
    }
  ]
}
```

### POST /api/v1/services
Create a new service.

**Request Body:**
```json
{
  "name": "Specialized Consultation",
  "description": "Detailed consultation for specific conditions",
  "category": "consultation",
  "duration": 60,
  "price": 1000.00,
  "pricingType": "fixed",
  "onlineAvailable": true,
  "offlineAvailable": true,
  "maxAdvanceBookingDays": 30,
  "bufferTime": 15
}
```

### PUT /api/v1/services/:id
Update service details.

### DELETE /api/v1/services/:id
Deactivate a service.

### GET /api/v1/services/public/:professionalId
Get public services for appointment booking.

## 💳 Invoice & Payment Endpoints

### GET /api/v1/invoices
Get all invoices.

**Query Parameters:**
- `status`: Filter by status
- `clientId`: Filter by client
- `startDate`: Date range start
- `endDate`: Date range end
- `page`: Page number
- `limit`: Results per page

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 2001,
      "invoiceNumber": "INV-2024-001",
      "clientId": 789,
      "client": {
        "firstName": "Jane",
        "lastName": "Doe",
        "email": "jane@example.com"
      },
      "issueDate": "2024-01-01",
      "dueDate": "2024-01-15",
      "subtotal": 1000.00,
      "taxRate": 18.00,
      "taxAmount": 180.00,
      "discountAmount": 0.00,
      "totalAmount": 1180.00,
      "currency": "INR",
      "status": "sent",
      "paymentTerms": "Net 15",
      "items": [
        {
          "id": 3001,
          "description": "General Consultation",
          "quantity": 2,
          "unitPrice": 500.00,
          "totalPrice": 1000.00,
          "appointmentId": 1001
        }
      ],
      "payments": [
        {
          "id": 4001,
          "amount": 1180.00,
          "status": "completed",
          "paymentMethod": "stripe",
          "paymentDate": "2024-01-02T14:30:00Z"
        }
      ],
      "createdAt": "2024-01-01T12:00:00Z"
    }
  ]
}
```

### POST /api/v1/invoices
Create a new invoice.

**Request Body:**
```json
{
  "clientId": 789,
  "dueDate": "2024-01-15",
  "paymentTerms": "Net 15",
  "taxRate": 18.00,
  "discountAmount": 0.00,
  "notes": "Payment due within 15 days",
  "items": [
    {
      "appointmentId": 1001,
      "serviceId": 111,
      "description": "General Consultation",
      "quantity": 1,
      "unitPrice": 500.00
    }
  ]
}
```

### GET /api/v1/invoices/:id
Get specific invoice details.

### PUT /api/v1/invoices/:id
Update invoice.

### POST /api/v1/invoices/:id/send
Send invoice to client.

### GET /api/v1/invoices/:id/pdf
Download invoice PDF.

### POST /api/v1/invoices/:id/payments
Record a payment.

**Request Body:**
```json
{
  "amount": 1180.00,
  "paymentMethod": "stripe",
  "transactionId": "txn_123456",
  "paymentDate": "2024-01-02T14:30:00Z",
  "notes": "Online payment via Stripe"
}
```

### POST /api/v1/payments/stripe/create-intent
Create Stripe payment intent.

**Request Body:**
```json
{
  "invoiceId": 2001,
  "amount": 1180.00,
  "currency": "inr"
}
```

### POST /api/v1/payments/stripe/confirm
Confirm Stripe payment.

## 💬 Communication Endpoints

### GET /api/v1/communications/conversations
Get all conversations for user.

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "participantId": 789,
      "participant": {
        "firstName": "Jane",
        "lastName": "Doe",
        "profileImage": "profile.jpg"
      },
      "lastMessage": {
        "id": 5001,
        "message": "Thank you for the consultation",
        "createdAt": "2024-01-01T15:30:00Z",
        "isRead": true
      },
      "unreadCount": 0,
      "lastActivity": "2024-01-01T15:30:00Z"
    }
  ]
}
```

### GET /api/v1/communications/conversations/:userId
Get conversation with specific user.

**Query Parameters:**
- `page`: Page number
- `limit`: Messages per page

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 5001,
      "senderId": 123,
      "receiverId": 789,
      "message": "Thank you for the consultation",
      "messageType": "text",
      "priority": "normal",
      "isRead": true,
      "readAt": "2024-01-01T15:31:00Z",
      "appointmentId": 1001,
      "attachments": [],
      "createdAt": "2024-01-01T15:30:00Z"
    }
  ]
}
```

### POST /api/v1/communications/send
Send a message.

**Request Body:**
```json
{
  "receiverId": 789,
  "message": "Your test results are ready",
  "messageType": "text",
  "priority": "normal",
  "appointmentId": 1001
}
```

### POST /api/v1/communications/:id/mark-read
Mark message as read.

### POST /api/v1/communications/send-email
Send email communication.

**Request Body:**
```json
{
  "to": "client@example.com",
  "subject": "Appointment Reminder",
  "message": "Your appointment is scheduled for tomorrow",
  "template": "appointment_reminder",
  "appointmentId": 1001
}
```

## 📄 Document Management Endpoints

### GET /api/v1/documents
Get documents for authenticated user.

**Query Parameters:**
- `clientId`: Filter by client
- `appointmentId`: Filter by appointment
- `documentType`: Filter by type
- `category`: Filter by category
- `page`: Page number
- `limit`: Results per page

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 6001,
      "uploaderId": 123,
      "clientId": 789,
      "appointmentId": 1001,
      "fileName": "prescription_20240101.pdf",
      "originalName": "Prescription.pdf",
      "fileSize": 204800,
      "mimeType": "application/pdf",
      "documentType": "prescription",
      "category": "medical",
      "description": "Prescription for cold symptoms",
      "isEncrypted": true,
      "downloadCount": 5,
      "isPublic": false,
      "expiresAt": null,
      "createdAt": "2024-01-01T16:00:00Z"
    }
  ]
}
```

### POST /api/v1/documents/upload
Upload documents.

**Request (Multipart Form):**
```
files: [File, File, ...]
clientId: 789
appointmentId: 1001
documentType: "prescription"
category: "medical"
description: "Test results"
isPublic: false
```

### GET /api/v1/documents/:id
Get document details.

### GET /api/v1/documents/:id/download
Download document file.

### PUT /api/v1/documents/:id
Update document metadata.

### DELETE /api/v1/documents/:id
Delete document.

### POST /api/v1/documents/:id/share
Share document with specific users.

**Request Body:**
```json
{
  "userIds": [789, 790],
  "permissions": ["read", "download"],
  "expiresAt": "2024-12-31T23:59:59Z",
  "message": "Sharing test results"
}
```

## 📊 Dashboard & Analytics Endpoints

### GET /api/v1/dashboard/overview
Get dashboard overview data.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "statistics": {
      "totalClients": 150,
      "totalAppointments": 850,
      "monthlyRevenue": 75000.00,
      "pendingInvoices": 5,
      "todayAppointments": 8,
      "weeklyAppointments": 35
    },
    "upcomingAppointments": [
      {
        "id": 1002,
        "clientName": "John Smith",
        "service": "General Consultation",
        "startTime": "2024-01-02T10:00:00Z",
        "type": "online"
      }
    ],
    "recentInvoices": [
      {
        "id": 2002,
        "invoiceNumber": "INV-2024-002",
        "clientName": "Jane Doe",
        "amount": 1180.00,
        "status": "pending"
      }
    ],
    "recentCommunications": [
      {
        "id": 5002,
        "senderName": "Alice Johnson",
        "message": "Thank you for the consultation",
        "createdAt": "2024-01-01T17:00:00Z"
      }
    ]
  }
}
```

### GET /api/v1/reports/revenue
Get revenue reports.

**Query Parameters:**
- `period`: "daily", "weekly", "monthly", "yearly"
- `startDate`: Report start date
- `endDate`: Report end date
- `groupBy`: "service", "client", "date"

### GET /api/v1/reports/appointments
Get appointment analytics.

### GET /api/v1/reports/clients
Get client analytics.

## 🔔 Notification Endpoints

### GET /api/v1/notifications
Get user notifications.

**Query Parameters:**
- `type`: Filter by notification type
- `isRead`: Filter by read status
- `page`: Page number
- `limit`: Results per page

### POST /api/v1/notifications/:id/mark-read
Mark notification as read.

### POST /api/v1/notifications/mark-all-read
Mark all notifications as read.

### DELETE /api/v1/notifications/:id
Delete notification.

## ⚙️ Business Settings Endpoints

### GET /api/v1/settings
Get business settings.

### PUT /api/v1/settings
Update business settings.

**Request Body:**
```json
{
  "businessHours": {
    "monday": {"start": "09:00", "end": "17:00", "isAvailable": true},
    "tuesday": {"start": "09:00", "end": "17:00", "isAvailable": true}
  },
  "appointmentSettings": {
    "defaultDuration": 30,
    "bufferTime": 15,
    "maxAdvanceBooking": 30,
    "autoConfirm": true
  },
  "invoiceSettings": {
    "defaultPaymentTerms": "Net 15",
    "taxRate": 18.00,
    "invoicePrefix": "INV",
    "autoSend": true
  },
  "notificationSettings": {
    "emailReminders": true,
    "smsReminders": true,
    "reminderTime": 24
  }
}
```

## 📱 WebSocket Events

### Real-time Communication Events

#### Client Events:
- `join_room`: Join conversation room
- `send_message`: Send real-time message
- `typing_start`: User started typing
- `typing_stop`: User stopped typing
- `mark_online`: User is online
- `mark_offline`: User is offline

#### Server Events:
- `new_message`: Receive new message
- `message_read`: Message marked as read
- `user_typing`: Other user typing
- `user_online`: User came online
- `user_offline`: User went offline
- `appointment_reminder`: Real-time appointment reminder
- `appointment_update`: Appointment status change
- `payment_received`: Payment confirmation

### Example WebSocket Usage
```javascript
// Client-side connection
const socket = io('/api/v1/ws', {
  auth: {
    token: 'jwt_token_here'
  }
});

// Join conversation room
socket.emit('join_room', {
  conversationId: 'user_123_user_789'
});

// Send message
socket.emit('send_message', {
  receiverId: 789,
  message: 'Hello!',
  messageType: 'text'
});

// Listen for new messages
socket.on('new_message', (data) => {
  console.log('New message:', data);
});
```

## 🔒 Rate Limiting

### Rate Limits by Endpoint Category:
- **Authentication**: 5 requests per minute
- **File Upload**: 10 requests per minute
- **General API**: 100 requests per minute
- **Search/Reports**: 50 requests per minute
- **Real-time Chat**: 200 requests per minute

## 📋 Error Codes

### Authentication Errors:
- `AUTH_REQUIRED`: Authentication required
- `INVALID_TOKEN`: Invalid or expired token
- `INSUFFICIENT_PERMISSIONS`: Insufficient permissions

### Validation Errors:
- `VALIDATION_ERROR`: Request validation failed
- `INVALID_FORMAT`: Invalid data format
- `MISSING_REQUIRED_FIELD`: Required field missing

### Business Logic Errors:
- `APPOINTMENT_CONFLICT`: Appointment time conflict
- `INSUFFICIENT_BALANCE`: Insufficient account balance
- `SERVICE_UNAVAILABLE`: Service not available
- `CLIENT_NOT_FOUND`: Client not found

### System Errors:
- `INTERNAL_ERROR`: Internal server error
- `DATABASE_ERROR`: Database operation failed
- `EXTERNAL_SERVICE_ERROR`: External service unavailable

This comprehensive API specification covers all the functionality required for ProConnectHub, providing a solid foundation for frontend and backend development. 