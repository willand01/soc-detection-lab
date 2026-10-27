# Windows Filtering Platform Investigation

**Exercise date:** October 20, 2026  
**Status:** Event collection observed; custom-rule end-to-end validation pending

## Objective

Observe Windows Filtering Platform (WFP) packet-block events in Wazuh after a controlled TCP connection attempt from the Kali lab endpoint to the Windows lab endpoint.

## Scope and Topology

- Source: `SOC-KALI` — `10.0.2.15`
- Destination: `SOC-WIN11` — `10.0.2.3`
- Destination port: TCP `4444`
- SIEM: Wazuh on `SOC-SIEM` — `10.0.2.4`
- Virtual network: isolated VirtualBox NAT Network `SOC Lab` (`10.0.2.0/24`)

## Method

1. Enabled Windows Filtering Platform packet-drop auditing.
2. Configured Wazuh to collect Windows Security Event IDs `5152` and `5157`.
3. Validated the configuration XML.
4. Generated a controlled TCP probe from Kali to Windows on port `4444`.
5. Searched Wazuh for Event ID `5152` and destination port `4444`.
6. Inspected the decoded event fields and investigated the parent Wazuh rule.

## Findings

The Wazuh search returned two matching records. The observed record included source IP `10.0.2.15`, destination IP `10.0.2.3`, destination port `4444`, protocol `6` (TCP), and filtering origin `Stealth`.

These observations support successful generation and ingestion of the relevant Windows filtering event and demonstrate that it can be searched in Wazuh.

## Detection Engineering

Custom rule `100510` was drafted with level `7`, parent rule `60104`, and a description identifying a WFP packet blocked from the Kali lab source. XML validation succeeded.

**Important validation distinction:** XML validity and a matching parent rule do not prove that the custom rule fired on a real event. The rule is considered pending until a fresh test produces an alert attributed to `100510` in Wazuh.

## Next Steps

- Verify the final rule XML and committed file path.
- Generate a fresh controlled test event.
- Search the resulting alert by rule ID `100510`.
- Capture separate evidence for the raw Windows event and the custom-rule alert, if generated.
- Update this report with the final outcome and any tuning.

## Security Context

This was a controlled test inside the lab network. A blocked connection attempt is not, on its own, evidence of compromise. Event IDs `5152` and `5157` represent different WFP block event types; validate the exact event ID and decoded fields for each investigation.
