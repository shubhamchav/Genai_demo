# SonarQube Troubleshooting Guide

## SonarQube Not Starting
- Check if service is running
- Run: sonarservice.bat start
- Check logs in SonarQube logs folder

## Database Connection Failed
- Verify PostgreSQL is running
- Check sonar.properties configuration
- Validate username/password

## Port Issue
- Ensure port 9000 is free
- Check firewall settings

## Permission Issues
- Verify DB privileges
- Re-run GRANT commands
