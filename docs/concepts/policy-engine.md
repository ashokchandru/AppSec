# Policy Engine

The Policy Engine automates decisions based on scan results and vulnerability data.

## How It Works
1. A scan or event triggers evaluation  
2. Policy rules are checked in order  
3. Matching rules trigger actions  

## Supported Conditions
- Severity > threshold  
- Dependency risk score  
- Application tags  
- Connector events  

## Supported Actions
- Block deployment  
- Send Slack/Email alerts  
- Create tickets  
- Auto-assign vulnerabilities  
- Require approval  

## Example Rule
if severity.high >= 1:
block_deployment()
The Policy Engine enables governance at scale.
