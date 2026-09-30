# Implementation progress

## Why the design changed

The original plan used prebuilt x86 virtual appliances. The development host uses Apple Silicon (ARM64), so the lab was adapted rather than forcing an unsupported architecture:

- Wazuh runs in Docker Desktop, which can run the required containers through emulation when necessary.
- Ubuntu remains in VirtualBox as a monitored endpoint.
- pfSense is being provisioned in UTM because its FreeBSD base is more compatible with Apple Silicon there than in the original VirtualBox setup.

This is a design decision, not a shortcut: it reflects a real-world constraint of matching security tooling to host hardware.

## Completed

### Wazuh platform

- Downloaded the Wazuh Docker deployment resources.
- Generated deployment certificates.
- Started the single-node Wazuh stack.
- Reached the Wazuh Dashboard after acknowledging the local TLS warning.

### Endpoint foundation

- Created an Ubuntu virtual machine.
- Configured host-only networking for reliable administration from the macOS host.
- Confirmed remote administration connectivity to the Ubuntu VM.

### Firewall foundation

- Created the pfSense VM in UTM.
- Assigned separate WAN and LAN interfaces.
- Configured the LAN interface as the internal lab gateway.

## Next milestone: endpoint telemetry

1. Install and enroll the Wazuh agent on Ubuntu.
2. Confirm the endpoint appears as **Active** in Wazuh.
3. Generate a normal, benign event on Ubuntu.
4. Find that event in Wazuh Threat Hunting.
5. Capture a sanitized screenshot and add it to this repository.

## Planned milestones

| Milestone | Evidence of completion |
| --- | --- |
| Ubuntu agent enrollment | Active agent plus searchable endpoint events |
| pfSense integration | Searchable firewall syslog events in Wazuh |
| Suricata integration | EVE JSON alert visible in Wazuh |
| File Integrity Monitoring | Create, modify, and delete events detected in a selected test folder |
| Windows/Sysmon telemetry | Sysmon process and authentication events searchable in Wazuh |
| Detection validation | Documented, isolated lab simulation and corresponding detection investigation |

## Repository hygiene

Before publishing any screenshot, log, configuration, or diagram, redact credentials, API keys, public IPs, hostnames, personally identifiable information, and any sensitive local network details.


## September 30, 2026 verification

After starting Docker Desktop and the Ubuntu VM, agent 001 reported Active. Actual endpoint rootcheck events appeared in the manager alert file. A separate synthetic failed-login log matched rule 5710 through wazuh-logtest. See the [visual evidence](demo/README.md). The earlier endpoint-enrollment milestone above is now complete; Dashboard investigation, firewall syslog and Suricata demonstrations remain future work.
