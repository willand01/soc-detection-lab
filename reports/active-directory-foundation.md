# Active Directory Foundation Lab

**Exercise date:** October 23, 2026  
**Status:** Domain join and domain sign-in verified

## Objective

Create a small Active Directory environment for entry-level helpdesk practice and future identity-focused security investigations.

## Environment

- Domain: `adlab.local`
- NetBIOS domain: `ADLAB`
- Domain controller: `AD-DC01` — `10.0.2.5` during setup
- Windows client: `AD-CLIENT01` — `10.0.2.6` observed during setup
- Hypervisor: VirtualBox
- Network: `SOC Lab` NAT Network

## Tasks Completed

1. Installed Windows Server 2022 Evaluation with Desktop Experience.
2. Configured Active Directory Domain Services and DNS.
3. Promoted `AD-DC01` to a domain controller for `adlab.local`.
4. Checked domain-controller advertising and queried the domain's SRV records; the result referenced `ad-dc01.adlab.local`.
5. Joined the Windows client to the domain.
6. Created a lab user and security group `GG_Helpdesk_Lab`.
7. Added the lab user to the group.
8. Troubleshot a sign-in issue and verified domain sign-in worked.

## Skills Demonstrated

- Windows Server and basic network setup
- Active Directory Domain Services and DNS concepts
- Domain joining and domain authentication
- User provisioning and security-group membership
- Basic authentication troubleshooting and verification

## Evidence to Add

- Domain controller and client VM inventory
- Domain/network diagram
- Screenshot showing domain membership
- Screenshot showing the lab user's group membership
- A helpdesk-style ticket with issue, troubleshooting steps, resolution, and verification

Ensure screenshots contain no passwords, recovery codes, tokens, or personal credentials.

## Next Exercises

- Practice account unlock and password-reset workflows using lab-only accounts.
- Test group membership changes and verify access behavior.
- Create a mock helpdesk ticket for each task.
- Explore available Windows identity events and determine how they could be forwarded to the SIEM for investigation.

## Resource and Safety Notes

The environment is for isolated training, not production use. Use unique lab-only credentials and power off VMs when not needed to conserve host resources. Observed IP addresses may change if DHCP leases or network configuration change.
