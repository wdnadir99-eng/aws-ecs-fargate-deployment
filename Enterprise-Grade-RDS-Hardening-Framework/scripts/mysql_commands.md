# MySQL Commands for RDS Hardening Project

## User Management & Authentication Commands

### Create IAM Database User
```sql
-- Create user for IAM authentication
CREATE USER 'iam_app_user' IDENTIFIED WITH AWSAuthenticationPlugin AS 'RDS';
```

-- Grant specific permissions to application database
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON secureapp.* TO 'iam_app_user';
```

-- Alternative: Grant all privileges (use cautiously)

-- GRANT ALL PRIVILEGES ON secureapp.* TO 'iam_app_user';
```

###Enforce SSL Connection
-- Force SSL for specific user
```
ALTER USER 'iam_app_user' REQUIRE SSL;
```

-- Force SSL for all users (global)
-- ALTER USER '%' REQUIRE SSL;


### Verify User Creation:

-- Check user exists and authentication method
```
SELECT user, host, plugin, ssl_type 
FROM mysql.user 
WHERE user = 'iam_app_user';

-- List all users and their authentication methods
SELECT user, host, plugin, ssl_type FROM mysql.user;

### Security Testing Commands
### Password Policy Validation

-- Test weak password (should fail with MEDIUM policy)
CREATE USER 'weak_user'@'%' IDENTIFIED BY 'weak';

-- Test strong password (should succeed)
CREATE USER 'strong_user'@'%' IDENTIFIED BY 'StrongPass123!';

-- Check password validation settings
SHOW VARIABLES LIKE 'validate_password%';

### SSL/TLS Connection Verification

-- Check current connection encryption
SHOW STATUS LIKE 'Ssl_cipher';

-- Verify SSL is being used (non-empty result means SSL active)
SELECT * FROM performance_schema.session_status 
WHERE VARIABLE_NAME = 'Ssl_cipher';

-- Check if user requires SSL
SELECT user, ssl_type FROM mysql.user;


### Database Operations Commands
###Basic Database Operations
-- Create application database
CREATE DATABASE secureapp;

-- Show all databases
SHOW DATABASES;

-- Use specific database
USE secureapp;

-- Show tables in current database
SHOW TABLES;

##Connection & Session Management

-- Show current connections
SHOW PROCESSLIST;

-- Show current user and connection details
SELECT USER(), CURRENT_USER();

-- Kill specific connection (replace ID from SHOW PROCESSLIST)
KILL [connection_id];

### Security Configuration Commands
### Check Security Parameters

-- Verify secure transport requirement
SHOW VARIABLES LIKE 'require_secure_transport';

-- Check local_infile setting
SHOW VARIABLES LIKE 'local_infile';

-- Verify secure_file_priv setting
SHOW VARIABLES LIKE 'secure_file_priv';

-- Check name resolution setting
SHOW VARIABLES LIKE 'skip_name_resolve';

### User Privilege Management
### View User Privileges

-- Show grants for specific user
SHOW GRANTS FOR 'iam_app_user';

-- Show grants for current user
SHOW GRANTS;

-- Detailed privilege information
SELECT * FROM mysql.user WHERE user = 'iam_app_user';

### Revoke Permissions (if needed)

-- Revoke specific permissions
REVOKE INSERT, UPDATE ON secureapp.* FROM 'iam_app_user';

-- Revoke all permissions
REVOKE ALL PRIVILEGES ON secureapp.* FROM 'iam_app_user';

-- Drop user completely
DROP USER 'iam_app_user';

## Performance and Maintenance
System Status

-- Show database version
SELECT VERSION();

-- Show system variables
SHOW VARIABLES;

-- Show status information
SHOW STATUS;

-- Check storage engine information
SHOW ENGINES;

### Table Operations (for application development)

-- Create sample table
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert sample data
INSERT INTO users (username, email) VALUES ('testuser', 'test@example.com');

-- Query data
SELECT * FROM users;

