# Wazuh SOC Homelab — Lab Journal

This journal records controlled security monitoring, detection engineering, and endpoint administration exercises completed in an isolated VirtualBox lab.

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

---

## Lab 2: Windows Filtering Platform Event Collection and Blocked-Traffic Investigation

**Date:** October 20, 2026

### Objective
Configure and validate collection of Windows Filtering Platform (WFP) block events, then investigate a controlled TCP connection attempt from the Kali lab endpoint to the Windows endpoint.

### Environment
- SIEM: Wazuh
- Windows endpoint: `SOC-WIN11` — `10.0.2.3`
- Kali endpoint: `SOC-KALI` — `10.0.2.15`
- SIEM endpoint: `SOC-SIEM` — `10.0.2.4`
- Virtual network: `SOC Lab` NAT Network (`10.0.2.0/24`)
- Test destination port: TCP `4444`

### Work Performed
1. Enabled Windows Filtering Platform packet-drop auditing after finding that the relevant auditing subcategory was not enabled.
2. Updated Wazuh Windows event collection configuration to include Security Event IDs `5152` and `5157`.
3. Validated the Wazuh configuration XML before restarting or relying on the manager configuration.
4. Generated a controlled TCP probe from Kali (`10.0.2.15`) toward Windows (`10.0.2.3`) on port `4444`.
5. Queried Wazuh for Event ID `5152` and destination port `4444`.
6. Reviewed the event fields and investigated the parent rule used by the Windows event.
7. Drafted custom Wazuh rule `100510` to describe a Windows Filtering Platform packet blocked from the Kali lab endpoint.

### Observed Results
- Wazuh search returned two matching records for Event ID `5152` with destination port `4444`.
- The observed event identified source `10.0.2.15`, destination `10.0.2.3`, destination port `4444`, and protocol `6` (TCP).
- The event included the filtering origin value `Stealth`.
- The custom rule configuration used ID `100510`, level `7`, and parent rule `60104`.
- XML validation succeeded, and the parent-rule relationship was investigated.

### Interpretation
The results support that the Windows filtering event was generated and ingested into Wazuh, and that the event could be searched using its ID and destination port. This is evidence of event collection and investigation—not, by itself, proof that the custom rule produced a final alert. End-to-end validation of rule `100510` remains a follow-up item.

A blocked connection attempt in this isolated lab is a controlled test, not proof of compromise. Event IDs `5152` and `5157` describe different WFP blocking scenarios; confirm the event's actual ID and fields during investigation rather than treating them as interchangeable.

### Detection Rule Intent
The custom rule was intended to identify a WFP packet-block event associated with the Kali lab source and provide a clear analyst-facing description. Rule logic should be tested against the exact decoded fields in the event, and the resulting alert should be verified in Wazuh before calling the detection complete.

### Follow-up
- Confirm the custom rule file location and full rule XML are committed to the repository.
- Generate a fresh controlled test event and verify whether rule `100510` fires in Wazuh.
- Capture a screenshot of the matching raw event and, separately, the custom-rule alert if it fires.
- Record any rule tuning and the final validation result.

---

## Lab 3: Active Directory Domain and Helpdesk Administration

**Date:** October 23, 2026

### Objective
Build a small Active Directory environment to practice common entry-level IT support tasks and establish a foundation for future identity-related security investigations.

### Environment
- Hypervisor: VirtualBox
- Domain controller: `AD-DC01`
- Windows client: `AD-CLIENT01` (initially displayed as `DESKTOP-GM2SM8A` before renaming)
- Active Directory DNS domain: `adlab.local`
- NetBIOS domain name: `ADLAB`
- Lab network: `SOC Lab` NAT Network
- Domain controller address used during setup: `10.0.2.5`
- Client address observed during setup: `10.0.2.6`

### Work Performed
1. Installed Windows Server 2022 Evaluation with Desktop Experience for the domain controller.
2. Configured the server's lab network settings and installed/configured Active Directory Domain Services and DNS.
3. Promoted the server to a domain controller for `adlab.local`.
4. Validated domain-controller health, including successful advertising checks and an SRV lookup referencing `ad-dc01.adlab.local`.
5. Prepared the Windows client and joined it to `adlab.local`.
6. Created a lab user account and a helpdesk security group named `GG_Helpdesk_Lab`.
7. Added the lab user to the group and resolved an account sign-in issue during testing.
8. Confirmed that domain sign-in worked.

### Outcomes and Skills Practiced
- Windows Server installation and basic network configuration
- Active Directory Domain Services and DNS fundamentals
- Domain join and domain authentication
- User and group provisioning
- Security-group membership management
- Basic sign-in troubleshooting and validation

### Security and Lab Notes
This is a deliberately small, isolated training environment. Use non-production credentials and avoid reusing personal passwords. Keep the domain controller and client powered off when not needed to conserve host resources.

### Follow-up
- Commit a simple domain/network diagram and VM inventory.
- Document a helpdesk ticket-style exercise covering account unlock, group membership, and verification.
- Add screenshots that demonstrate domain membership and user/group configuration without exposing passwords or other secrets.
- Later, connect identity events to the SOC monitoring workflow where feasible.

---

## Portfolio Documentation Standards

For each new exercise, record the objective, environment, exact steps, observed evidence, interpretation, limitations, and follow-up tasks. Distinguish between a configuration that validates successfully and a detection that has been demonstrated end-to-end. Do not include passwords, tokens, private keys, or other secrets in screenshots or repository files.
