# Redshift SOC

[svg](https://github.com/Mujtabakhancy/redshift-soc#redshift-soc)

Redshift SOC is a self-contained, segmented Security Operations Center environment built on a single 16GB host. It combines a default-deny perimeter firewall (pfSense), centralized log collection (Splunk Enterprise), and endpoint telemetry (Sysmon, Windows/Linux logs) with a library of safe, MITRE ATT&CK-mapped attack simulations used to validate detection coverage end to end.

The repo covers the full lifecycle: the pre-build project charter and architecture rationale, the build log with real infrastructure issues diagnosed and resolved, the attack simulations run against the environment, and — as a forward extension — **Triarc**, an AI-assisted triage layer that correlates new alerts against known true/false positives.

**Hypervisor:** VMware Workstation Pro

## Documents

[svg](https://github.com/Mujtabakhancy/redshift-soc#documents)

| **File**                                                                                                                                           | **What it is**                                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Redshift-SOC-Project-Charter.docx`](https://github.com/Mujtabakhancy/redshift-soc/blob/main/Redshift-SOC-Project-Charter.docx)                   | Pre-build project charter & technical design document — objectives, requirements, architecture decisions and rationale, risk assessment, phased roadmap |
| [`SOC-Home-Lab-Documentation.docx`](https://github.com/Mujtabakhancy/redshift-soc/blob/main/SOC-Home-Lab-Documentation.docx)                       | Build log — Step 1 through Step 10, with real troubleshooting and root causes documented alongside the intended process                                 |
| [`Redshift_SOC_Attack_Simulation_Report.docx`](https://github.com/Mujtabakhancy/redshift-soc/blob/main/Redshift_SOC_Attack_Simulation_Report.docx) | Step 11 — MITRE ATT&CK-mapped attack simulations, detection engineering, Splunk telemetry validation, and detection coverage assessment                 |

## Architecture

[svg](https://github.com/Mujtabakhancy/redshift-soc#architecture)

```text
                         [ Attacker Segment ]
                          Kali Linux (192.168.10.50)
                               |
                     OPT1 Interface (192.168.10.1)
                               |
                    [ Perimeter Firewall - pfSense ]
                    /          |          \
              WAN (NAT)   LAN Interface
              Internet     (192.168.20.1)
                               |
                 [ Protected Segment - 192.168.20.0/24 ]
                   Windows 10 Endpoint | Ubuntu Server (Apache)
                               |
                   [ Centralized Logging - Splunk Enterprise ]
                        (Host-resident, 192.168.20.5)
                               |
                [ Planned: Triarc AI Triage Layer ]
```

**svg**

## Network & IP Plan

[svg](https://github.com/Mujtabakhancy/redshift-soc#network--ip-plan)

| **Device**           | **IP Address**          | **Role**                           |
| -------------------- | ----------------------- | ---------------------------------- |
| pfSense WAN          | DHCP (VMware NAT range) | Internet uplink                    |
| pfSense LAN gateway  | 192.168.20.1            | Internal gateway                   |
| pfSense OPT1 gateway | 192.168.10.1            | Attacker segment gateway           |
| Windows 10 Endpoint  | 192.168.20.10           | Victim / monitored endpoint        |
| Ubuntu Server        | 192.168.20.20           | Victim / Apache web server         |
| Kali Linux           | 192.168.10.50           | Attacker machine                   |
| Host laptop (Splunk) | 192.168.20.5            | Log collection (Splunk Enterprise) |

## Build Status

[svg](https://github.com/Mujtabakhancy/redshift-soc#build-status)

| **Step** | **Description**                                              | **Status** |
| -------- | ------------------------------------------------------------ | ---------- |
| 1        | Lab Architecture & Environment Planning                      | ✅ Complete |
| 2        | Hypervisor Installation & Base Network Configuration         | ✅ Complete |
| 3        | pfSense Firewall Deployment (3-interface design)             | ✅ Complete |
| 4        | Windows Endpoint Deployment                                  | ✅ Complete |
| 5        | Ubuntu Linux Server Deployment                               | ✅ Complete |
| 6        | Kali Linux Attacker Deployment                               | ✅ Complete |
| 7        | Apache Web Server Deployment                                 | ✅ Complete |
| 8        | Firewall Rule Verification & Traffic Logging                 | ✅ Complete |
| 9        | Splunk Enterprise Deployment                                 | ✅ Complete |
| 10       | Log Forwarding — Sysmon, Splunk Universal Forwarder & Syslog | ✅ Complete |
| 11       | Attack Simulation & Detection Validation (MITRE ATT&CK)      | ✅ Complete |
| 12       | Triarc — AI-Assisted Alert Triage                            | ⬜ Planned  |

## Skills Demonstrated

[svg](https://github.com/Mujtabakhancy/redshift-soc#skills-demonstrated)

Network segmentation design, firewall deployment and rule engineering (pfSense), Windows/Linux endpoint administration, Sysmon configuration with an industry-standard detection ruleset, Splunk Enterprise administration (licensing, indexes, forwarders), systematic network troubleshooting (packet capture, firewall state-table analysis, host-based firewall scoping), MITRE ATT&CK-mapped attack simulation, detection engineering, Splunk-based detection validation, and resource-constrained infrastructure design.

## Notable Troubleshooting Highlights

[svg](https://github.com/Mujtabakhancy/redshift-soc#notable-troubleshooting-highlights)

* Diagnosed a pfSense WAN configuration issue (`Block private networks and loopback addresses`) that silently dropped all NAT'd outbound and inter-segment traffic — found via layered packet capture and firewall state-table inspection, not guesswork.
* Identified a genuine IP address conflict between the host laptop and pfSense's own gateway addresses on two virtual networks.
* Diagnosed a Splunk Universal Forwarder startup failure on Windows down to a service-account permissions issue, cross-referenced against Splunk's official documentation.
* Traced a syslog delivery failure between two independently-confirmed-healthy endpoints (pfSense actively sending, Splunk actively listening) to a missing host firewall rule — a pattern that recurred across three different ports/protocols in this build.

## Future Improvements

[svg](https://github.com/Mujtabakhancy/redshift-soc#future-improvements)

* Build **Triarc**: an AI-assisted triage layer that ingests fired alerts and recommends a True/False Positive classification with supporting rationale, validated against a labeled set of true and false positives generated during attack simulation
* Add FortiGate VM alongside pfSense for enterprise NGFW experience
