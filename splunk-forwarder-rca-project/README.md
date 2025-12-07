# Splunk Universal Forwarder RCA Project Summary

This project details the **Root Cause Analysis (RCA)** performed to resolve an issue where a Windows Universal Forwarder (UF) was deployed but failed to send any data (reported as "Zero Events") to the central Splunk Indexer.

### 🎯 Problem Statement

A Windows Server was configured with a Universal Forwarder, but diagnostic steps confirmed that **no events were being indexed** for the Windows Security, Application, or System logs.

### 🔍 Root Cause Analysis (RCA)

The issue was traced to a **misconfiguration within the UF's local configuration files**, specifically the `inputs.conf`.

1.  **Diagnostic Findings:** While network connectivity to the Indexer was confirmed, a review of the UF's configuration showed the necessary data input stanzas were either **commented out** or had the `disabled = 1` setting. This prevented the UF from reading the event logs entirely.

2.  **Specific Root Cause:** The configuration stanzas for monitoring the event logs were either completely absent or contained the line `disabled = 1`.

### ✅ Solution and Remediation

The issue was resolved by correcting the `inputs.conf` file to explicitly enable monitoring of the required event logs and assigning the correct destination index:

1.  The necessary stanzas (`[WinEventLog://Security]`, etc.) were enabled.
2.  The line `disabled = 1` was changed to **`disabled = 0`**.
3.  The correct destination index (`index = windows`) was assigned.

Upon service restart, events immediately began flowing from the Universal Forwarder to the Splunk Indexer.

### 🖼️ Evidence and Configuration

* **Configuration Files:** The final, correct `inputs.conf` and `outputs.conf` are provided in the **`config_files`** subdirectory.
* **Visual Evidence:** Screenshots documenting the misconfigured file state, the correction, and the final successful data indexing are located in the **`screenshots`** subdirectory.