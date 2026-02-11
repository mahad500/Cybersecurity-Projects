# Alert Triage with Splunk – Linux Brute Force Analysis

## Overview
This project documents my work investigating a Linux brute force alert in a SOC environment using Splunk. I analyzed login events and user activity to determine whether the alert was malicious.

## Objectives
- Analyze Linux authentication logs in Splunk  
- Identify brute-force activity and successful logins  
- Investigate suspicious internal IP activity  
- Document key findings  

## Tools Used
- Splunk (TryHackMe lab environment)  
- Linux authentication logs (`linux-alert` index)  

## Key Findings
- Alert originated from an internal IP, suggesting network access.  
- Multiple failed login attempts observed, including non-existent users.  
- **john.smith** had over **500 login attempts**, confirming brute force activity.  
- Successful login for `john.smith`; attacker escalated to **root** and created **system-utm** for persistence.  

## Screenshots / Observations
*Insert screenshots of Splunk dashboards here:*  
- `alert_overview.png`  
- `failed_login_attempts.png`  
- `brute_force_user_attempts.png`  
- `successful_login.png`  

## Outcome
This task helped me practice alert triage, detect brute-force attacks, and analyze Linux authentication logs in a SOC environment.

