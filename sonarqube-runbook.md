SonarQube Runbook (Windows with PostgreSQL)

1. Project Information

- SonarQube Version: 26.1.0 LTA
- PostgreSQL Version: 14.19
- VM IP Address: 172.27.59.66
- Setup By: Shubham Chavan (ALM Team)
- Admin Access: Onkar

---

2. Overview

This document provides steps to install, configure, and operate SonarQube with PostgreSQL on a Windows system.

---

3. Prerequisites

- Windows machine (x64)
- Java 21 or compatible version
- PostgreSQL installed
- SonarQube extracted locally

---

4. Install PostgreSQL

1. Download and install PostgreSQL.
2. During installation:
   - Set a password for the postgres user.
   - Use default port 5432.

---

5. Configure Environment Variables

Add PostgreSQL bin directory to system PATH:

C:\Program Files\PostgreSQL\<version>\bin

---

6. Access PostgreSQL Shell

Open SQL Shell (psql) and enter:

- Server: localhost
- Database: postgres
- Port: 5432
- Username: postgres
- Password: <your_password>

---

7. Create Database and User

CREATE DATABASE sonarqube;
CREATE USER sonar WITH ENCRYPTED PASSWORD 'admin';
ALTER ROLE sonar SET client_encoding TO 'utf8';
ALTER ROLE sonar SET default_transaction_isolation TO 'read committed';
ALTER ROLE sonar SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;

---

8. Test Database Connection

psql -U sonar -d sonarqube -h localhost -p 5432

---

9. Verify Connection

SELECT current_database();
SELECT current_user;

Expected output:

- sonarqube
- sonar

---

10. Configure SonarQube

Edit the following file:

<SONARQUBE_HOME>\conf\sonar.properties

Add:

sonar.jdbc.username=sonar
sonar.jdbc.password=admin
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube

---

11. Login as PostgreSQL Admin

psql -U postgres -d sonarqube -h localhost -p 5432

---

12. Assign Permissions

GRANT ALL ON SCHEMA public TO sonar;
ALTER SCHEMA public OWNER TO sonar;
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;
ALTER DATABASE sonarqube OWNER TO sonar;

---

13. Verify Permissions (Optional)

\dn+

---

14. Configure SonarQube Service

Navigate to:

sonarqube-26.1.0\bin\windows-x86-64

Run (as Administrator):

sonarservice.bat install
sonarservice.bat start

---

15. Operational Commands

Start service:

sonarservice.bat start

Stop service:

sonarservice.bat stop

Restart service:

sonarservice.bat stop
sonarservice.bat start

---

16. Access SonarQube

Open in browser:

http://172.27.59.66:9000

Default credentials:

- Username: admin
- Password: admin

---

17. Notes

- Ensure PostgreSQL service is running before starting SonarQube.
- Verify port 9000 is available.
- Check logs if service fails to start.

---

18. Summary

This runbook covers installation, configuration, database setup, and service operations for SonarQube.
