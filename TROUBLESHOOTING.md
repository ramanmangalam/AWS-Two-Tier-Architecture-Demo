# Troubleshooting Guide for AWS Two-Tier Architecture

This document provides solutions to common issues you may encounter while deploying or using the AWS Two-Tier Architecture.

## 1. Auto Scaling Group Configuration
- **Issue**: If you configure the Auto Scaling Group (ASG) in a public subnet, instances may not connect as expected.
- **Solution**: Ensure that the ASG is set up in the correct private subnet. Instances in a public subnet may have different routing and security configurations that could prevent proper connectivity.

## 2. SSH Connection Issues
- **Issue**: You might encounter issues when trying to connect to instances without appropriate privileges.
- **Solution**: Always use `sudo` when executing commands on the instances to ensure you have the necessary permissions. For example:
  ```bash
  sudo <command>
  
## 3. Security Group Configuration
- **Issue**:Applications may not be accessible if security group settings are incorrect.
- **Solution**: Double-check that the security group allows inbound traffic on the necessary ports (e.g., port 22 for SSH and port 8000 for the application).
-  Ensure that rules are properly defined to avoid connectivity issues.

