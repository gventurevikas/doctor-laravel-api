# 🗄️ MedProfile Pro - Medical Database Schema

## 📋 Overview
Complete database schema design for MedProfile Pro medical platform with HIPAA-compliant data structures, patient records management, electronic health records (EHR), and medical practice administration.

## 🏥 Medical Database Design Principles

### HIPAA Compliance Requirements
- **Data Encryption**: All patient data encrypted at rest using AES-256
- **Access Controls**: Role-based permissions with audit trails
- **Data Retention**: Configurable retention policies for medical records
- **Backup Security**: Encrypted backups with medical data protection
- **Audit Logging**: Complete audit trails for all medical data access

### Medical Data Integrity
- **ACID Compliance**: MySQL transactions for medical data consistency
- **Foreign Key Constraints**: Referential integrity for medical relationships
- **Medical Indexes**: Optimized queries for large patient datasets
- **Data Validation**: Medical-specific constraints and validations

## 📊 Medical Database Schema

### Core Medical Tables

#### 1. Doctors Table
```sql
CREATE TABLE doctors (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    
    -- Basic Information
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    middle_name VARCHAR(100),
    title VARCHAR(20), -- Dr., Prof., etc.
    gender ENUM('male', 'female', 'other'),
    date_of_birth DATE,
    phone VARCHAR(20),
    
    -- Medical Credentials
    medical_license_number VARCHAR(50) UNIQUE NOT NULL,
    license_state VARCHAR(10) NOT NULL,
    license_expiry_date DATE NOT NULL,
    medical_license_verified BOOLEAN DEFAULT FALSE,
    
    -- Specializations
    primary_specialty VARCHAR(100) NOT NULL,
    sub_specialties JSON, -- Array of sub-specializations
    board_certifications JSON, -- Array of certifications
    
    -- Education & Experience
    medical_school VARCHAR(200),
    graduation_year YEAR,
    residency_programs JSON, -- Array of residency details
    fellowship_programs JSON, -- Array of fellowship details
    years_of_experience INT,
    
    -- Professional Details
    hospital_affiliations JSON, -- Array of hospital details
    insurance_plans_accepted JSON, -- Array of insurance plans
    languages_spoken JSON, -- Array of languages
    
    -- Contact Information
    office_address TEXT,
    office_city VARCHAR(100),
    office_state VARCHAR(50),
    office_zip VARCHAR(10),
    office_phone VARCHAR(20),
    office_fax VARCHAR(20),
    
    -- Profile & Website
    profile_photo_url VARCHAR(500),
    biography TEXT,
    education_background TEXT,
    awards_achievements TEXT,
    publications TEXT,
    research_interests TEXT,
    
    -- Practice Settings
    consultation_fee DECIMAL(10,2),
    follow_up_fee DECIMAL(10,2),
    average_consultation_time INT, -- in minutes
    emergency_consultations BOOLEAN DEFAULT FALSE,
    telemedicine_enabled BOOLEAN DEFAULT TRUE,
    
    -- Account Status
    email_verified BOOLEAN DEFAULT FALSE,
    two_factor_enabled BOOLEAN DEFAULT FALSE,
    account_status ENUM('active', 'inactive', 'suspended') DEFAULT 'active',
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    last_login_at TIMESTAMP NULL,
    
    INDEX idx_medical_license (medical_license_number),
    INDEX idx_specialty (primary_specialty),
    INDEX idx_location (office_city, office_state),
    INDEX idx_email (email)
);
```

#### 2. Patients Table
```sql
CREATE TABLE patients (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    patient_id VARCHAR(20) UNIQUE NOT NULL, -- Human-readable ID
    doctor_id CHAR(36) NOT NULL,
    
    -- Basic Information
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    middle_name VARCHAR(100),
    date_of_birth DATE NOT NULL,
    gender ENUM('male', 'female', 'other') NOT NULL,
    ssn VARCHAR(11), -- Encrypted
    
    -- Contact Information
    email VARCHAR(255),
    phone VARCHAR(20),
    alternate_phone VARCHAR(20),
    address TEXT,
    city VARCHAR(100),
    state VARCHAR(50),
    zip_code VARCHAR(10),
    country VARCHAR(100) DEFAULT 'United States',
    
    -- Emergency Contact
    emergency_contact_name VARCHAR(200),
    emergency_contact_relationship VARCHAR(100),
    emergency_contact_phone VARCHAR(20),
    emergency_contact_phone2 VARCHAR(20),
    
    -- Medical Information
    blood_type ENUM('A+', 'A-', 'B+', 'B-', 'AB+', 'AB-', 'O+', 'O-', 'Unknown'),
    height_cm DECIMAL(5,2),
    weight_kg DECIMAL(5,2),
    bmi DECIMAL(4,2) GENERATED ALWAYS AS (weight_kg / ((height_cm/100) * (height_cm/100))) STORED,
    
    -- Insurance Information
    insurance_provider VARCHAR(200),
    insurance_policy_number VARCHAR(100),
    insurance_group_number VARCHAR(100),
    insurance_phone VARCHAR(20),
    
    -- Medical Alerts
    allergies JSON, -- Array of allergy objects
    chronic_conditions JSON, -- Array of chronic conditions
    current_medications JSON, -- Array of current medications
    medical_alerts JSON, -- Array of critical alerts
    
    -- Family History
    family_medical_history JSON, -- Structured family history
    
    -- Account Information
    patient_portal_access BOOLEAN DEFAULT TRUE,
    preferred_language VARCHAR(50) DEFAULT 'English',
    communication_preference ENUM('email', 'sms', 'phone', 'mail') DEFAULT 'email',
    
    -- Status
    patient_status ENUM('active', 'inactive', 'deceased') DEFAULT 'active',
    registration_date DATE DEFAULT (CURRENT_DATE),
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (doctor_id) REFERENCES doctors(id) ON DELETE CASCADE,
    INDEX idx_patient_id (patient_id),
    INDEX idx_doctor_patients (doctor_id),
    INDEX idx_patient_name (last_name, first_name),
    INDEX idx_patient_dob (date_of_birth),
    INDEX idx_patient_status (patient_status)
);
```

#### 3. Medical Appointments Table
```sql
CREATE TABLE medical_appointments (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    appointment_number VARCHAR(20) UNIQUE NOT NULL,
    doctor_id CHAR(36) NOT NULL,
    patient_id CHAR(36) NOT NULL,
    
    -- Appointment Details
    appointment_date DATE NOT NULL,
    appointment_time TIME NOT NULL,
    duration_minutes INT DEFAULT 30,
    appointment_type ENUM('consultation', 'follow_up', 'procedure', 'emergency', 'physical_exam', 'telemedicine') NOT NULL,
    
    -- Status and Management
    status ENUM('scheduled', 'confirmed', 'in_progress', 'completed', 'cancelled', 'no_show') DEFAULT 'scheduled',
    cancellation_reason TEXT,
    cancelled_by ENUM('doctor', 'patient', 'system'),
    cancelled_at TIMESTAMP NULL,
    
    -- Appointment Details
    chief_complaint TEXT, -- Patient's primary concern
    appointment_notes TEXT, -- Pre-appointment notes
    special_instructions TEXT,
    
    -- Location and Type
    consultation_type ENUM('in_person', 'telemedicine', 'phone') DEFAULT 'in_person',
    office_location VARCHAR(200),
    telemedicine_link VARCHAR(500),
    
    -- Billing Information
    consultation_fee DECIMAL(10,2),
    insurance_copay DECIMAL(10,2),
    payment_status ENUM('pending', 'paid', 'insurance_pending', 'insurance_paid') DEFAULT 'pending',
    
    -- Reminders
    reminder_sent BOOLEAN DEFAULT FALSE,
    reminder_sent_at TIMESTAMP NULL,
    confirmation_sent BOOLEAN DEFAULT FALSE,
    confirmation_sent_at TIMESTAMP NULL,
    
    -- Follow-up
    follow_up_required BOOLEAN DEFAULT FALSE,
    follow_up_in_days INT,
    follow_up_instructions TEXT,
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    checked_in_at TIMESTAMP NULL,
    checked_out_at TIMESTAMP NULL,
    
    FOREIGN KEY (doctor_id) REFERENCES doctors(id) ON DELETE CASCADE,
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    INDEX idx_appointment_date (appointment_date, appointment_time),
    INDEX idx_doctor_appointments (doctor_id, appointment_date),
    INDEX idx_patient_appointments (patient_id, appointment_date),
    INDEX idx_appointment_status (status),
    INDEX idx_appointment_type (appointment_type)
);
```

#### 4. Medical Records (EHR) Table
```sql
CREATE TABLE medical_records (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    patient_id CHAR(36) NOT NULL,
    doctor_id CHAR(36) NOT NULL,
    appointment_id CHAR(36),
    
    -- Record Type and Classification
    record_type ENUM('consultation', 'progress_note', 'procedure_note', 'discharge_summary', 'lab_result', 'imaging_report', 'referral') NOT NULL,
    record_date DATE NOT NULL,
    record_time TIME NOT NULL,
    
    -- SOAP Notes Structure
    subjective TEXT, -- Patient's description of symptoms
    objective TEXT, -- Doctor's observations and findings
    assessment TEXT, -- Medical diagnosis and assessment
    plan TEXT, -- Treatment plan and next steps
    
    -- Vital Signs
    blood_pressure_systolic INT,
    blood_pressure_diastolic INT,
    heart_rate INT,
    temperature_celsius DECIMAL(4,2),
    respiratory_rate INT,
    oxygen_saturation INT,
    height_cm DECIMAL(5,2),
    weight_kg DECIMAL(5,2),
    
    -- Clinical Findings
    diagnosis_codes JSON, -- ICD-10 codes
    procedure_codes JSON, -- CPT codes
    symptoms JSON, -- Array of symptoms
    physical_examination TEXT,
    
    -- Treatment and Recommendations
    treatment_provided TEXT,
    medications_prescribed JSON, -- Array of medications
    lifestyle_recommendations TEXT,
    activity_restrictions TEXT,
    diet_instructions TEXT,
    
    -- Follow-up and Referrals
    follow_up_required BOOLEAN DEFAULT FALSE,
    follow_up_timeframe VARCHAR(100),
    referrals_made JSON, -- Array of referral details
    tests_ordered JSON, -- Array of tests/labs ordered
    
    -- Record Management
    record_status ENUM('draft', 'final', 'amended', 'corrected') DEFAULT 'draft',
    confidentiality_level ENUM('normal', 'restricted', 'very_restricted') DEFAULT 'normal',
    
    -- Digital Signature and Verification
    doctor_signature TEXT, -- Digital signature
    signature_timestamp TIMESTAMP,
    verified_by_doctor BOOLEAN DEFAULT FALSE,
    verification_timestamp TIMESTAMP,
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES doctors(id) ON DELETE CASCADE,
    FOREIGN KEY (appointment_id) REFERENCES medical_appointments(id) ON DELETE SET NULL,
    INDEX idx_patient_records (patient_id, record_date),
    INDEX idx_doctor_records (doctor_id, record_date),
    INDEX idx_record_type (record_type),
    INDEX idx_record_date (record_date)
);
```

#### 5. Prescriptions Table
```sql
CREATE TABLE prescriptions (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    prescription_number VARCHAR(20) UNIQUE NOT NULL,
    patient_id CHAR(36) NOT NULL,
    doctor_id CHAR(36) NOT NULL,
    appointment_id CHAR(36),
    medical_record_id CHAR(36),
    
    -- Medication Details
    medication_name VARCHAR(200) NOT NULL,
    generic_name VARCHAR(200),
    medication_strength VARCHAR(100),
    medication_form ENUM('tablet', 'capsule', 'liquid', 'injection', 'topical', 'inhaler', 'patch', 'other') NOT NULL,
    
    -- Dosage Instructions
    dosage_amount VARCHAR(100) NOT NULL,
    dosage_frequency VARCHAR(100) NOT NULL, -- e.g., "twice daily", "every 8 hours"
    dosage_duration VARCHAR(100), -- e.g., "7 days", "2 weeks"
    total_quantity INT NOT NULL,
    quantity_unit VARCHAR(50), -- tablets, ml, etc.
    
    -- Administration Instructions
    administration_route ENUM('oral', 'topical', 'injection', 'inhalation', 'rectal', 'other') DEFAULT 'oral',
    administration_instructions TEXT,
    food_instructions VARCHAR(200), -- "with food", "on empty stomach"
    special_instructions TEXT,
    
    -- Prescription Management
    refills_authorized INT DEFAULT 0,
    refills_remaining INT DEFAULT 0,
    date_prescribed DATE NOT NULL,
    start_date DATE,
    end_date DATE,
    
    -- Clinical Information
    indication TEXT, -- Why medication was prescribed
    contraindications TEXT,
    drug_interactions_checked BOOLEAN DEFAULT TRUE,
    allergy_checked BOOLEAN DEFAULT TRUE,
    
    -- Pharmacy Information
    pharmacy_name VARCHAR(200),
    pharmacy_phone VARCHAR(20),
    pharmacy_address TEXT,
    transmitted_to_pharmacy BOOLEAN DEFAULT FALSE,
    transmission_date TIMESTAMP NULL,
    
    -- Status and Compliance
    prescription_status ENUM('active', 'completed', 'cancelled', 'expired') DEFAULT 'active',
    patient_compliance ENUM('excellent', 'good', 'fair', 'poor', 'unknown') DEFAULT 'unknown',
    
    -- Electronic Prescription
    e_prescription_id VARCHAR(100),
    electronic_signature TEXT,
    dea_number VARCHAR(20), -- For controlled substances
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    filled_date TIMESTAMP NULL,
    
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES doctors(id) ON DELETE CASCADE,
    FOREIGN KEY (appointment_id) REFERENCES medical_appointments(id) ON DELETE SET NULL,
    FOREIGN KEY (medical_record_id) REFERENCES medical_records(id) ON DELETE SET NULL,
    INDEX idx_patient_prescriptions (patient_id, date_prescribed),
    INDEX idx_doctor_prescriptions (doctor_id, date_prescribed),
    INDEX idx_prescription_status (prescription_status),
    INDEX idx_medication_name (medication_name)
);
```

#### 6. Lab Results Table
```sql
CREATE TABLE lab_results (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    patient_id CHAR(36) NOT NULL,
    doctor_id CHAR(36) NOT NULL,
    appointment_id CHAR(36),
    order_id VARCHAR(50),
    
    -- Lab Test Information
    test_name VARCHAR(200) NOT NULL,
    test_category VARCHAR(100), -- Hematology, Chemistry, Microbiology, etc.
    test_code VARCHAR(50), -- LOINC code
    specimen_type VARCHAR(100), -- Blood, Urine, etc.
    
    -- Results
    result_value VARCHAR(500),
    result_unit VARCHAR(50),
    reference_range VARCHAR(200),
    result_status ENUM('normal', 'abnormal', 'critical', 'pending') NOT NULL,
    
    -- Test Details
    collection_date DATE NOT NULL,
    collection_time TIME,
    result_date DATE,
    result_time TIME,
    
    -- Laboratory Information
    lab_name VARCHAR(200),
    lab_technician VARCHAR(200),
    pathologist VARCHAR(200),
    
    -- Clinical Context
    clinical_notes TEXT,
    doctor_interpretation TEXT,
    follow_up_required BOOLEAN DEFAULT FALSE,
    critical_value BOOLEAN DEFAULT FALSE,
    
    -- File Attachments
    result_document_url VARCHAR(500),
    result_pdf_path VARCHAR(500),
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES doctors(id) ON DELETE CASCADE,
    FOREIGN KEY (appointment_id) REFERENCES medical_appointments(id) ON DELETE SET NULL,
    INDEX idx_patient_labs (patient_id, collection_date),
    INDEX idx_doctor_labs (doctor_id, collection_date),
    INDEX idx_test_name (test_name),
    INDEX idx_result_status (result_status)
);
```

#### 7. Medical Billing Table
```sql
CREATE TABLE medical_billing (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    invoice_number VARCHAR(20) UNIQUE NOT NULL,
    patient_id CHAR(36) NOT NULL,
    doctor_id CHAR(36) NOT NULL,
    appointment_id CHAR(36),
    
    -- Billing Details
    service_date DATE NOT NULL,
    billing_date DATE DEFAULT (CURRENT_DATE),
    due_date DATE,
    
    -- Services and Charges
    services_provided JSON, -- Array of services with CPT codes
    procedure_codes JSON, -- Array of procedure codes
    diagnosis_codes JSON, -- Array of ICD-10 codes
    
    -- Financial Information
    subtotal DECIMAL(10,2) NOT NULL,
    tax_amount DECIMAL(10,2) DEFAULT 0.00,
    discount_amount DECIMAL(10,2) DEFAULT 0.00,
    total_amount DECIMAL(10,2) NOT NULL,
    
    -- Insurance Information
    insurance_provider VARCHAR(200),
    insurance_claim_number VARCHAR(100),
    insurance_authorization_number VARCHAR(100),
    insurance_copay DECIMAL(10,2) DEFAULT 0.00,
    insurance_deductible DECIMAL(10,2) DEFAULT 0.00,
    insurance_coverage_percentage DECIMAL(5,2) DEFAULT 0.00,
    insurance_amount_expected DECIMAL(10,2) DEFAULT 0.00,
    
    -- Payment Information
    patient_amount_due DECIMAL(10,2) NOT NULL,
    amount_paid DECIMAL(10,2) DEFAULT 0.00,
    payment_method ENUM('cash', 'check', 'credit_card', 'debit_card', 'insurance', 'payment_plan') NULL,
    payment_status ENUM('unpaid', 'partial', 'paid', 'overdue', 'written_off') DEFAULT 'unpaid',
    
    -- Status and Management
    billing_status ENUM('draft', 'sent', 'payment_pending', 'insurance_pending', 'completed', 'cancelled') DEFAULT 'draft',
    collection_status ENUM('current', 'overdue_30', 'overdue_60', 'overdue_90', 'collection_agency') DEFAULT 'current',
    
    -- Communication
    invoice_sent_date DATE,
    reminder_count INT DEFAULT 0,
    last_reminder_date DATE,
    
    -- Notes
    billing_notes TEXT,
    payment_notes TEXT,
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    paid_at TIMESTAMP NULL,
    
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES doctors(id) ON DELETE CASCADE,
    FOREIGN KEY (appointment_id) REFERENCES medical_appointments(id) ON DELETE SET NULL,
    INDEX idx_patient_billing (patient_id, service_date),
    INDEX idx_doctor_billing (doctor_id, service_date),
    INDEX idx_payment_status (payment_status),
    INDEX idx_billing_date (billing_date)
);
```

#### 8. Medical Staff Table
```sql
CREATE TABLE medical_staff (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    doctor_id CHAR(36) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    
    -- Basic Information
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    phone VARCHAR(20),
    
    -- Role and Permissions
    role ENUM('nurse', 'medical_assistant', 'receptionist', 'office_manager', 'admin') NOT NULL,
    permissions JSON, -- Array of specific permissions
    
    -- Professional Information
    license_number VARCHAR(100),
    certification VARCHAR(200),
    hire_date DATE,
    
    -- Account Status
    account_status ENUM('active', 'inactive', 'suspended') DEFAULT 'active',
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    last_login_at TIMESTAMP NULL,
    
    FOREIGN KEY (doctor_id) REFERENCES doctors(id) ON DELETE CASCADE,
    INDEX idx_doctor_staff (doctor_id),
    INDEX idx_staff_role (role),
    INDEX idx_staff_email (email)
);
```

### Audit and Compliance Tables

#### 9. Medical Audit Log Table
```sql
CREATE TABLE medical_audit_log (
    id CHAR(36) PRIMARY KEY DEFAULT (UUID()),
    
    -- User Information
    user_id CHAR(36) NOT NULL,
    user_type ENUM('doctor', 'staff', 'patient', 'system') NOT NULL,
    user_email VARCHAR(255),
    
    -- Action Details
    action ENUM('create', 'read', 'update', 'delete', 'login', 'logout', 'access_denied') NOT NULL,
    entity_type ENUM('patient', 'appointment', 'medical_record', 'prescription', 'lab_result', 'billing') NOT NULL,
    entity_id CHAR(36),
    
    -- Patient Context (for HIPAA compliance)
    patient_id CHAR(36),
    patient_name VARCHAR(255), -- Encrypted
    
    -- System Information
    ip_address VARCHAR(45),
    user_agent TEXT,
    session_id VARCHAR(255),
    
    -- Change Details
    old_values JSON,
    new_values JSON,
    
    -- Compliance Information
    hipaa_authorization BOOLEAN DEFAULT TRUE,
    access_reason TEXT,
    
    -- Timestamp
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_user_audit (user_id, created_at),
    INDEX idx_patient_audit (patient_id, created_at),
    INDEX idx_entity_audit (entity_type, entity_id),
    INDEX idx_action_audit (action, created_at)
);
```

## 🔧 Medical Database Views

### Patient Summary View
```sql
CREATE VIEW patient_summary_view AS
SELECT 
    p.id,
    p.patient_id,
    p.first_name,
    p.last_name,
    p.date_of_birth,
    p.gender,
    p.phone,
    p.email,
    p.blood_type,
    p.patient_status,
    
    -- Latest appointment
    (SELECT appointment_date FROM medical_appointments 
     WHERE patient_id = p.id AND status = 'completed' 
     ORDER BY appointment_date DESC LIMIT 1) as last_appointment_date,
    
    -- Active prescriptions count
    (SELECT COUNT(*) FROM prescriptions 
     WHERE patient_id = p.id AND prescription_status = 'active') as active_prescriptions_count,
    
    -- Upcoming appointments count
    (SELECT COUNT(*) FROM medical_appointments 
     WHERE patient_id = p.id AND appointment_date >= CURRENT_DATE AND status = 'scheduled') as upcoming_appointments_count
     
FROM patients p
WHERE p.patient_status = 'active';
```

### Doctor Schedule View
```sql
CREATE VIEW doctor_schedule_view AS
SELECT 
    ma.id,
    ma.appointment_date,
    ma.appointment_time,
    ma.duration_minutes,
    ma.appointment_type,
    ma.status,
    
    -- Patient information
    p.patient_id,
    CONCAT(p.first_name, ' ', p.last_name) as patient_name,
    p.phone as patient_phone,
    
    -- Doctor information
    d.id as doctor_id,
    CONCAT(d.first_name, ' ', d.last_name) as doctor_name
    
FROM medical_appointments ma
JOIN patients p ON ma.patient_id = p.id
JOIN doctors d ON ma.doctor_id = d.id
WHERE ma.appointment_date >= CURRENT_DATE
ORDER BY ma.appointment_date, ma.appointment_time;
```

## 🔐 Medical Security and Encryption

### Encryption Configuration
```sql
-- Enable MySQL encryption at rest
ALTER TABLE patients ENCRYPTION='Y';
ALTER TABLE medical_records ENCRYPTION='Y';
ALTER TABLE prescriptions ENCRYPTION='Y';
ALTER TABLE lab_results ENCRYPTION='Y';
ALTER TABLE medical_audit_log ENCRYPTION='Y';

-- SSL Configuration for connections
SET GLOBAL require_secure_transport=ON;
```

### HIPAA Compliance Procedures
```sql
-- Patient data access procedure with audit logging
DELIMITER //
CREATE PROCEDURE AccessPatientData(
    IN p_user_id CHAR(36),
    IN p_patient_id CHAR(36),
    IN p_access_reason TEXT
)
BEGIN
    DECLARE v_user_type ENUM('doctor', 'staff', 'patient', 'system');
    
    -- Log the access attempt
    INSERT INTO medical_audit_log (
        user_id, patient_id, action, entity_type, 
        access_reason, created_at
    ) VALUES (
        p_user_id, p_patient_id, 'read', 'patient',
        p_access_reason, NOW()
    );
    
    -- Return patient data (would include actual data selection)
    SELECT 'Patient data accessed and logged' as result;
END //
DELIMITER ;
```

This comprehensive medical database schema provides a robust foundation for HIPAA-compliant patient data management, electronic health records, and medical practice administration while ensuring data security and audit compliance. 