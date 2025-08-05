# 🔧 Medical API Environment Configuration

## Overview

This document describes the environment configuration setup for the Medical API project. Multiple environment files have been created to support different deployment scenarios.

## 📁 Environment Files Created

| File | Purpose | Environment |
|------|---------|-------------|
| `env.development` | Local development | Development |
| `env.staging` | Staging/testing | Staging |
| `env.production` | Production deployment | Production |
| `env.testing` | Automated testing | Testing |
| `env.example` | Template for new environments | Template |

## 🗄️ Database Configuration

All environment files are configured with your MySQL database:

```env
DB_CONNECTION=mysql
DB_HOST=198.204.241.250
DB_PORT=3306
DB_DATABASE=docuter_db
DB_USERNAME=staging_php
DB_PASSWORD="ASFGFSjkus12!@#"
```

## 🚀 Quick Setup

### 1. **Development Environment**
```bash
./scripts/setup-env.sh development
```

### 2. **Staging Environment**
```bash
./scripts/setup-env.sh staging
```

### 3. **Production Environment**
```bash
./scripts/setup-env.sh production
```

### 4. **Testing Environment**
```bash
./scripts/setup-env.sh testing
```

## 🔧 Manual Setup

If you prefer to set up manually:

```bash
# Copy the appropriate environment file
cp env.development .env

# Generate application key
php artisan key:generate

# Run migrations
php artisan migrate

# Clear caches
php artisan cache:clear
php artisan config:clear
```

## 📊 Environment-Specific Configurations

### **Development Environment**
- **Debug**: Enabled
- **Log Level**: Debug
- **Cache**: File-based
- **Queue**: Synchronous
- **Rate Limiting**: Relaxed (200 req/min)
- **Database**: `docuter_db`

### **Staging Environment**
- **Debug**: Enabled
- **Log Level**: Debug
- **Cache**: File-based
- **Queue**: Synchronous
- **Rate Limiting**: Moderate (120 req/min)
- **Database**: `docuter_db`

### **Production Environment**
- **Debug**: Disabled
- **Log Level**: Error only
- **Cache**: Redis
- **Queue**: Redis
- **Rate Limiting**: Strict (60 req/min)
- **Database**: `docuter_db`

### **Testing Environment**
- **Debug**: Enabled
- **Log Level**: Debug
- **Cache**: Array-based
- **Queue**: Synchronous
- **Rate Limiting**: Disabled
- **Database**: `docuter_db_test`

## 🔒 Security Configuration

All environments include HIPAA compliance settings:

```env
# HIPAA Compliance Settings
HIPAA_AUDIT_LOGGING_ENABLED=true
HIPAA_DATA_ENCRYPTION_ENABLED=true
HIPAA_ACCESS_CONTROL_ENABLED=true
HIPAA_TRANSMISSION_SECURITY_ENABLED=true

# Security Assessment Settings
FAIL_ON_SECURITY_ISSUES=false
SECURITY_LOG_LEVEL=debug
AUDIT_LOGGING_ENABLED=true
ENCRYPTION_ENABLED=true
```

## 📧 Mail Configuration

### **Development/Testing**
```env
MAIL_MAILER=log
MAIL_FROM_ADDRESS="dev@medicalapp.com"
```

### **Staging**
```env
MAIL_MAILER=log
MAIL_FROM_ADDRESS="staging@medicalapp.com"
```

### **Production**
```env
MAIL_MAILER=smtp
MAIL_FROM_ADDRESS="hello@medicalapp.com"
```

## 🗂️ File Storage Configuration

Each environment has separate file storage disks:

- **Development**: `medical_documents_dev`, `medical_images_dev`
- **Staging**: `medical_documents_staging`, `medical_images_staging`
- **Production**: `medical_documents`, `medical_images`
- **Testing**: `medical_documents_test`, `medical_images_test`

## 🔄 Cache Configuration

### **Development/Staging**
```env
CACHE_DRIVER=file
CACHE_TTL=1800
CACHE_PREFIX=medical_api_staging_
```

### **Production**
```env
CACHE_DRIVER=redis
CACHE_TTL=3600
CACHE_PREFIX=medical_api_
```

### **Testing**
```env
CACHE_DRIVER=array
CACHE_TTL=300
CACHE_PREFIX=medical_api_test_
```

## 📈 Monitoring Configuration

### **Development/Staging**
```env
MONITORING_ENABLED=true
HEALTH_CHECK_ENABLED=true
PERFORMANCE_MONITORING_ENABLED=true
DEBUG_BAR_ENABLED=true
TELESCOPE_ENABLED=true
```

### **Production**
```env
MONITORING_ENABLED=true
HEALTH_CHECK_ENABLED=true
PERFORMANCE_MONITORING_ENABLED=true
DEBUG_BAR_ENABLED=false
TELESCOPE_ENABLED=false
```

### **Testing**
```env
MONITORING_ENABLED=false
HEALTH_CHECK_ENABLED=true
PERFORMANCE_MONITORING_ENABLED=false
DEBUG_BAR_ENABLED=false
TELESCOPE_ENABLED=false
```

## 🛠️ Environment Setup Script

The `scripts/setup-env.sh` script automates the environment setup process:

### **Features**
- ✅ Copies appropriate environment file
- ✅ Generates application key
- ✅ Tests database connection
- ✅ Runs database migrations
- ✅ Seeds database (development/staging only)
- ✅ Clears application caches
- ✅ Shows environment information

### **Usage**
```bash
# Show help
./scripts/setup-env.sh --help

# Set up development environment
./scripts/setup-env.sh development

# Set up staging environment
./scripts/setup-env.sh staging

# Set up production environment
./scripts/setup-env.sh production

# Set up testing environment
./scripts/setup-env.sh testing
```

## 🔍 Environment Validation

After setup, validate your environment:

```bash
# Check environment
php artisan env

# Test database connection
php artisan migrate:status

# Check application health
php artisan health:check

# Run security tests
./scripts/security-assessment.sh --tests-only
```

## 📝 Environment Variables Reference

### **Core Laravel Variables**
- `APP_NAME` - Application name
- `APP_ENV` - Environment (local, staging, production, testing)
- `APP_DEBUG` - Debug mode
- `APP_URL` - Application URL
- `APP_KEY` - Application encryption key

### **Database Variables**
- `DB_CONNECTION` - Database driver (mysql)
- `DB_HOST` - Database host (198.204.241.250)
- `DB_PORT` - Database port (3306)
- `DB_DATABASE` - Database name (docuter_db)
- `DB_USERNAME` - Database username (staging_php)
- `DB_PASSWORD` - Database password

### **Medical API Specific Variables**
- `MEDICAL_API_VERSION` - API version
- `MEDICAL_API_ENVIRONMENT` - Environment type
- `MEDICAL_API_DEBUG` - API debug mode
- `MEDICAL_DOCUMENTS_DISK` - Medical documents storage
- `MEDICAL_IMAGES_DISK` - Medical images storage
- `MAX_FILE_SIZE` - Maximum file upload size

### **Security Variables**
- `HIPAA_AUDIT_LOGGING_ENABLED` - HIPAA audit logging
- `HIPAA_DATA_ENCRYPTION_ENABLED` - Data encryption
- `HIPAA_ACCESS_CONTROL_ENABLED` - Access control
- `FAIL_ON_SECURITY_ISSUES` - Security test failures
- `SECURITY_LOG_LEVEL` - Security logging level

## 🚨 Important Notes

### **Security Considerations**
1. **Never commit `.env` files** to version control
2. **Use strong passwords** in production
3. **Enable encryption** for sensitive data
4. **Regular security audits** are recommended

### **Database Considerations**
1. **Backup regularly** in production
2. **Use separate databases** for different environments
3. **Monitor performance** and optimize queries
4. **Implement proper indexing** for medical data

### **Deployment Considerations**
1. **Set appropriate permissions** for file storage
2. **Configure SSL/TLS** for production
3. **Set up monitoring** and alerting
4. **Implement backup strategies**

## 📞 Support

For environment setup issues:
- Check database connectivity
- Verify file permissions
- Review Laravel logs
- Run security assessment tests

---

*Last Updated: July 30, 2025*
*Document Owner: DevOps Team* 