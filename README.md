# SOC Detection Lab

Hands-on cybersecurity portfolio project demonstrating endpoint log collection, security event investigation, detection engineering, and Windows identity administration in an isolated VirtualBox lab.

## Lab Highlights

- **Wazuh SIEM:** monitored endpoint agents and centralized event review.
- **Linux authentication monitoring:** generated a controlled failed sudo authentication on Kali Purple and identified the Wazuh alert (`PAM: User login failed`, rule `5503`, level 5).
- **Windows Filtering Platform investigation:** enabled/collected Windows Security events `5152` and `5157`, generated a controlled TCP probe from Kali to Windows port `4444`, and investigated the event in Wazuh.
- **Detection engineering:** drafted custom Wazuh rule `100510` for a Windows filtering packet-block event. End-to-end firing of the custom rule remains a validation task and is not claimed as complete.
- **Active Directory administration:** built a Windows Server domain controller and Windows client for `adlab.local`, practiced domain join, user creation, security-group membership, and sign-in troubleshooting.

## Documentation

- [Lab Journal](Lab-Journal.md) — objectives, environment, procedures, observed results, interpretation, and follow-up tasks.

## Lab Environment

The environment uses VirtualBox virtual machines connected to an isolated NAT Network named `SOC Lab` (`10.0.2.0/24`).

| System | Role | Lab address |
|---|---|---|
| SOC-SIEM | Wazuh SIEM | `10.0.2.4` |
| SOC-WIN11 | Windows monitored endpoint | `10.0.2.3` |
| SOC-KALI | Kali Purple test endpoint | `10.0.2.15` |
| AD-DC01 | Active Directory domain controller | `10.0.2.5` |
| AD-CLIENT01 | Windows domain client | `10.0.2.6` observed during setup |

Addresses are lab observations and may change if network settings or DHCP leases change.

## Approach

Each exercise is performed in a controlled lab. Documentation separates observed evidence from interpretation and explicitly notes when a detection still needs end-to-end validation. No production systems are targeted.

## Planned Work

- Validate the custom Wazuh rule `100510` with a fresh event and confirm whether the alert fires.
- Add a network/VM architecture diagram and supporting screenshots.
- Complete helpdesk-style Active Directory exercises and document troubleshooting steps.
- Expand detection and investigation scenarios as the lab develops.
