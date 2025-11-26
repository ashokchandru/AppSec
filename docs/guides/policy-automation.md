# Policy Automation Guide

Policies help enforce security gates and automate decision-making across your applications.

## Creating a Policy
1. Navigate to **Policy Center → Create Policy**
2. Choose a template or create a custom rule
3. Define:
   - Conditions  
   - Severity thresholds  
   - Actions (notify, block, auto-ticket, etc.)  

## Typical Policies
### Block Deployment on Critical Vulns
severity.critical > 0
action: block


### Auto-Create Jira Ticket
severity.high > 0
action: create_ticket


### Notify Slack for New Findings
event: vulnerability.new
action: slack_notify


## Policy Execution Flow
1. A scan completes  
2. Policy engine evaluates all rules  
3. Actions trigger automatically  
