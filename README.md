# SPEAR for Ansible Automation Platform

SPEAR stands for "STIG Policy Evaluation & Automated Remediation". This repository contains Ansible playbooks designed to automate the process of running STIG (Security Technical Implementation Guide) compliance checks against various systems and uploading the results to a STIG Manager instance.

## Overview

The playbooks perform the following high-level steps:
1.  Connect to a target system (Linux, Windows, or Cisco IOS-XE).
2.  Run the DISA's scc or the Navy's `Evaluate-STIG` tool (if you install that binary) to perform a compliance scan.
3.  Generate a STIG checklist file (`.cklb`, `.ckl`, or XCCDF `.xml`).
4.  Transfer the checklist file to the Ansible controller.
5.  Upload directly to OpenRMF, upload to an NFS share, and/or use `stigman-watcher` to upload the checklist to STIG Manager.

## Playbooks

-   `linux_run_evaluate_stig.yml`: Runs a STIG evaluation on a target Linux machine.
-   `windows_run_evaluate_stig.yml`: Runs a STIG evaluation on a target Windows machine.
-   `cisco_run_evaluate_stig.yml`: Gathers configuration from a Cisco IOS-XE device, runs a STIG evaluation against the configuration on the Ansible controller, and uploads the results.

## Requirements

### Execution Environment

These playbooks are intended to be run within an Ansible Execution Environment that is running within Ansible Automation Platform. An execution environment with all the binaries necessary is uploaded and referenced within the repository where need be. That EE is located here:
[ghcr.io/itsallvoodoo/spear_ee](https://github.com/itsallvoodoo/ansible_spear/pkgs/container/spear_ee)

The execution environment will need to contain:
-   `stigman-watcher`, with the playbooks expecting its location to be `/opt/stigman-watcher/`.
-   For the Cisco playbook, the `Evaluate-STIG` tool must be unzipped in `/opt/Evaluate-STIG/`.
-   The required Ansible collections, which are defined in `collections/requirements.yml`.

### Target Systems

-   **Linux**: SSH access and credentials. `unzip` and `libicu` packages are required and will be installed by the playbook if missing.
-   **Windows**: WinRM access and credentials. PowerShell 7 is required and will be installed by the playbook if missing.
-   **Cisco**: SSH access and credentials with privileges to run `show tech-support`.

### Evaluate-STIG Tool

The `Evaluate-STIG.zip` file must be present in the `files/` directory of the applicable role (Linux, Windows, and Cisco). This tool is used to perform STIG evaluation if that is the tool you choose to use. It is a proprietary tool made by the US Navy and is not provided as part of this repository. DISA's SCC tool is provided in this repository and is the default tool to use. Either tool accomplishes the job of scanning the hosts.

## Configuration

The playbooks require configuration to connect to your STIG Manager instance. This is managed through Ansible variables. A target host is needed to install STIG Manager and OpenRMF, along with Keycloak for authentication.

### Todo

Need to document all of the variables that are needed in an austere environment

## Usage

### Todo

Describe usage
