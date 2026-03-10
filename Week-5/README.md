# **WEEK 5 — Sysmon & Advanced Windows Logging**

## Short Summary

Enterprise-grade Windows telemetry was configured by deploying Sysmon with the SwiftOnSecurity configuration and enabling PowerShell Script Block Logging. The Wazuh agent was updated to collect from the Sysmon event channel, establishing high-fidelity log ingestion for process, registry, DNS, and PowerShell activity.

Sysmon v15.15 was installed via elevated PowerShell and validated through Event Viewer, confirming immediate generation of process, registry, and DNS query events. The Wazuh agent `ossec.conf` was manually updated to include the Sysmon Operational channel and restarted to apply changes. Log flow was confirmed in the Wazuh dashboard with 26 Sysmon hits verified under Security Events.

This phase significantly increases the depth and quality of host telemetry feeding the SIEM, enabling detection use cases beyond basic Windows Security log coverage.

---

## Sysmon Installation

Sysmon v15.15 installed to `C:\Tools\Sysmon` with SwiftOnSecurity configuration:

```powershell
cd C:\Tools\Sysmon
.\Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

Config file: `sysmonconfig-export.xml` (SwiftOnSecurity, schema version 4.50)

---

## PowerShell Script Block Logging

Enabled via registry:

```powershell
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" /v EnableScriptBlockLogging /t REG_DWORD /d 1 /f
```

Verified with:

```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging"
```

---

## Wazuh Agent Configuration

The Sysmon Operational channel was not included in the default agent config and was added manually to `C:\Program Files (x86)\ossec-agent\ossec.conf`:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

Agent restarted to apply changes:

```powershell
Restart-Service -Name WazuhSvc
```

---

## Sysmon Event ID Validation

Events were deliberately triggered and verified to confirm Sysmon telemetry is functioning correctly.

### Event ID 1 — Process Create

Triggered by launching `notepad.exe` via PowerShell. Confirmed in Wazuh:

`data.win.system.eventID: 1`

### Event ID 13 — Registry Value Set

Triggered by writing a test registry key:

```powershell
reg add "HKCU\Software\SysmonTest" /v TestValue /t REG_SZ /d "hello" /f
```

Note: Raw field `data.win.system.eventID: 13` returned no results. Event surfaced via Wazuh rule instead:

`rule.id: 750`

### Event ID 22 — DNS Query

Triggered via `nslookup`. Confirmed present in Event Viewer (Sysmon Operational log) but did not surface in Wazuh dashboard. Sysmon is capturing DNS queries correctly — gap is at the Wazuh parsing/rule level. To be investigated in a later week.

---

## Screenshots

### Sysmon Installation (PowerShell output)

![](Pasted_image_20260310175708.png)

### Event Viewer — Sysmon Operational Channel

![](Pasted_image_20260310175511.png)

### Wazuh Dashboard — Sysmon Hits (66 events)

![](Pasted_image_20260310175444.png)

### Event ID 1 — Process Create (Wazuh)

![](Pasted_image_20260310174107.png)

### Event ID 13 — Registry Value Set (rule.id: 750, Wazuh)

![](Pasted_image_20260310174508.png)

### Event ID 22 — DNS Query (Event Viewer confirmation)

![](Pasted_image_20260310175028.png)

---

## Operational Skills Demonstrated

- Sysmon deployment and configuration with community ruleset
    
- PowerShell Script Block Logging via registry policy
    
- Wazuh agent log channel configuration
    
- Event Viewer validation of third-party logging tools
    
- SIEM log source expansion beyond default Windows channels
    
- Deliberate event generation for telemetry validation
    
- Identifying and documenting SIEM parsing gaps
