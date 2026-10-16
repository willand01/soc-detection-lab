# Wazuh SOC Homelab — Lab Journal

## Lab 1: Linux Authentication Failure Detection

**Date:** October 15, 2026

### Objective
Validate Wazuh event collection and detection of Linux authentication failures on a monitored endpoint.

### Environment
- SIEM: Wazuh
- Endpoint: Kali Purple 2025.2
- Agent name: `SOC-KALI`
- Virtualization: VirtualBox 7.2.18
- Lab network: `SOC Lab` NAT Network

### Procedure
1. Started the Wazuh SIEM and confirmed dashboard access.
2. Started Kali and verified the Wazuh agent was active and connected.
3. Confirmed authentication-related events were visible in Wazuh.
4. Ran `sudo -k` to invalidate the cached sudo authentication timestamp.
5. Ran `sudo -v` and entered an incorrect password once.
6. Searched Wazuh Events for the resulting authentication-failure event.

### Observed Results
- Event description: `PAM: User login failed`
- Rule ID: `5503`
- Rule level: `5`
- Agent: `SOC-KALI`

### Conclusion
The controlled authentication failure appeared in Wazuh and was associated with a detection rule. This validated the basic endpoint-to-SIEM event collection and investigation workflow.

A single failed authentication attempt is not, by itself, evidence of malicious activity.

### Evidence
- Screenshot of the expanded Wazuh authentication-failure event.

### Next Steps
- Test a small number of repeated authentication failures in the isolated lab.
- Determine whether additional alerts or correlation rules trigger.
- Document the event evidence and investigation findings.
