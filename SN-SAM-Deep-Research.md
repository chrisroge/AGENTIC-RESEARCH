# Key points
- SCCM can provide signfiicant SAM coverage with minimal disruption
- Cloud Discovery may provide additional coverage with minimal disruption
- Discovery beyond SCCM and Cloud Discovery will require a significant investiment in time and complexity with MID servers requiring line of sight across all multiple firewall layers, large coordination with infrasructure and security teams require and independent impact statements are required from each tower

## Focus 
Focus on optimizing SCCM and Azure Cloud Discovery for SAM by ensuring SCCM clients are installed on all devices. Perform a gap analysis to identify non-SCCM devices, using ACC Agent or ServiceNow Discovery as needed, with minimal firewall adjustments for agent-based methods.

### High Level discovery disruption
- Shazzam Probe detects open ports with no credentials (initial scan)
- Port probe follows to look for open ports with no credentials
- Classification probe completes the loop with varying credential requirements

### High Level Deliverables and Operational Debt
- ARB approval for overall effort, full participation from infrastructure and security teams
- FASM approved for various MID sources and data center subnet destinations
- PII process followed, Security Review, possibly legal
- Credentials for various application layers, service account placed in Windows Server Admin Group for example, various other credentials obtained as needed

Phase 1 - SCCM and Cloud Discovery for SAM
Sprint 0 seeks to outline steps needed to optimize SAM data from existing SCCM and Cloud Dicovery channels

Phase 2 - SAM Discovery
Sprint 0 seeks to outline steps needed to standup comprehensive SAM discovery, ARB, FASM, MID coverage planning etc.

### Credential examples
- Domain admin or local admin on Windows Servers
- SSH credentials to read system directories on Linux Servers
- Database specific credentials for usage metrics, example: Oracle access to specific table to track license consumption
- SNMP community strings to gain low level access to network devices for devices which might host embedded software
- OAuth credentials for cloud channels such as Adobe subscription and usage data
- Citrix or other such vendor specific credentials (SAP, Red Hat etc).

Why Privileged Credentials Are Needed
**Granular License Data**: SAM requires detailed data (e.g., installation paths, usage metrics, active sessions) to track license consumption accurately, often necessitating administrative or database-level access.
**Discovery Process**: ServiceNow Discovery uses probes (e.g., Shazzam, classification probes) that need credentials to access systems and retrieve software details (ServiceNow Discovery Configuration).
**Vendor-Specific Metrics**: Vendors like Oracle require access to specific data points (e.g., database tables) for compliance, which demands privileged credentials (Oracle Database Credentials).



### Focus on Gaps outside of SCCM and Cloud Discovery
- SCCM and Azure Cloud Discovery likely cover software from multiple vendors, not just Microsoft, if SCCM clients are installed.
- Gaps include devices without SCCM clients, especially non-Windows, and potentially SaaS apps.
- Devices without SCCM clients, especially non-Windows, are likely missed.
- SaaS applications or cloud-based software not installed locally may be overlooked.
- Unmanaged or rogue devices not in SCCM or Azure could be blind spots.
- Niche software not detected by SCCM due to non-standard installation.


# Research Gaps

Software Asset Management Gap Analysis Report
Objective
Identify potential gaps in Software Asset Management (SAM) using SCCM and Azure Cloud Discovery in ServiceNow, focusing on software vendor coverage, ACC Agent necessity, firewall implications, and blind spots.
Current Setup

SCCM 2016: Reports 8x daily from Windows servers and laptops.
Azure Cloud Discovery: Daily discovery from Azure subscriptions via ServiceNow Cloud Management plugin.
Environment: Mostly Azure-based, small dispersed on-prem footprint.
Focus: Software discovery and license reconciliation.

Analysis by Device Lane
1. SCCM Covered Devices

Coverage: Windows, Linux, Mac with SCCM clients installed.
Software Data: Collects installed applications from multiple vendors, not just Microsoft, via hardware inventory (SCCM Inventory).
Gaps: Devices without SCCM clients miss software inventory.
Firewall: Agent-based, no scanning; requires SQL access (port 1433) for MID server to SCCM database.

2. Cloud Discovery Covered Devices

Coverage: Azure resources (e.g., VMs) discovered via API.
Software Data: Relies on SCCM clients for software inventory; no direct software data from Cloud Discovery.
Gaps: Non-SCCM Azure VMs, especially Linux/Mac, lack software inventory.
Firewall: API-based, requires outbound HTTPS from MID server; no scanning.

3. On-Premises Devices

Coverage: SCCM-covered devices report software inventory; others require ServiceNow Discovery or ACC Agent.
Software Data: Non-SCCM devices need alternative methods for vendor-agnostic software data.
Gaps: Non-SCCM on-prem devices, especially non-Windows, are blind spots.
Firewall: ServiceNow Discovery requires firewall rules (e.g., SSH:22, WMI:135) from MID to target subnets.

Potential Gaps

Non-SCCM Devices: Any device without SCCM client (Windows, Linux, Mac) lacks software inventory.
Non-Windows Devices: Linux/Mac without SCCM clients need ACC Agent or Discovery.
SaaS Applications: SAM may miss cloud-based software not installed locally.
Software Detection: SCCM may miss niche or custom software not registered in standard locations.

Firewall and Scanning Implications

SCCM/ACC: Agent-based, devices phone home; no scanning or firewall changes needed beyond MID server access to SCCM database or ServiceNow instance.
Azure Cloud Discovery: API-based, no scanning; requires MID server outbound internet access.
ServiceNow Discovery: Active scanning for non-SCCM on-prem devices requires firewall rules from MID server source IPs to destination subnets, potentially numerous depending on network segmentation.

Recommendations

Optimize SCCM: Ensure SCCM clients are installed on all Windows, Linux, and Mac devices to maximize software inventory coverage (Mac Management).
Gap Analysis: Identify non-SCCM devices via network scans or manual audits; assess need for ACC Agent or ServiceNow Discovery.
ACC Agent: Deploy for non-SCCM Linux/Mac devices to collect software inventory without scanning (Agent Client Collector).
ServiceNow Discovery: Use for on-prem non-SCCM devices if ACC is impractical; plan firewall rules carefully.
SaaS Integration: Explore SAM integrations for SaaS apps if relevant.

Blind Spots

Unmanaged Devices: Rogue or unmanaged devices not in SCCM or Azure.
Dynamic Environments: Rapidly provisioned Azure resources without SCCM clients.
Complex Software: Software not detected by SCCM due to non-standard installation.

Conclusion
Optimizing SCCM and Azure Cloud Discovery provides robust SAM coverage for devices with SCCM clients, capturing multi-vendor software data. Gaps in non-SCCM devices can be addressed with ACC Agent or ServiceNow Discovery, with careful firewall planning for the latter. A gap analysis will ensure comprehensive software asset management.



**Key Points**
- SCCM and Azure Cloud Discovery likely cover software from multiple vendors, not just Microsoft, if SCCM clients are installed.
- ACC Agent may not be needed for Macs/Linux if SCCM clients are present, but useful for non-SCCM devices.
- Gaps include devices without SCCM clients, especially non-Windows, and potentially SaaS apps.
- No scanning for SCCM/Azure; ServiceNow Discovery requires firewall rules for on-prem scanning.

**Software Vendor Coverage**
SCCM likely collects software data from various vendors on Windows, Linux, and Mac with SCCM clients installed ([SCCM Inventory](https://mindmajix.com/sccm-inventory)). Azure Cloud Discovery focuses on resources, relying on SCCM for software data.

**ACC Agent Necessity**
If SCCM clients are on Macs/Linux, software inventory is likely covered, reducing ACC Agent need ([Mac Management](https://www.anoopcnair.com/sccm-mac-management-take-decision-in-10-mins/)). ACC is useful for devices without SCCM clients.

**Firewall and Scanning**
SCCM and ACC are agent-based, requiring no scanning or firewall changes. ServiceNow Discovery for on-prem needs firewall rules (e.g., SSH/WMI) from MID servers to target subnets ([ServiceNow Discovery](https://www.servicenow.com/community/itom-articles/discovery-101/ta-p/2314968)).

**Recommendation**
Optimize SCCM and Azure Cloud Discovery, ensuring SCCM clients on all devices. Conduct gap analysis for non-SCCM devices, considering ACC or Discovery.


# Software Asset Management Gap Analysis Report

## Objective
Identify potential gaps in Software Asset Management (SAM) using SCCM and Azure Cloud Discovery in ServiceNow, focusing on software vendor coverage, ACC Agent necessity, firewall implications, and blind spots.

## Current Setup
- **SCCM 2016**: Reports 8x daily from Windows servers and laptops.
- **Azure Cloud Discovery**: Daily discovery from Azure subscriptions via ServiceNow Cloud Management plugin.
- **Environment**: Mostly Azure-based, small dispersed on-prem footprint.
- **Focus**: Software discovery and license reconciliation.

## Analysis by Device Lane

### 1. SCCM Covered Devices
- **Coverage**: Windows, Linux, Mac with SCCM clients installed.
- **Software Data**: Collects installed applications from multiple vendors, not just Microsoft, via hardware inventory ([SCCM Inventory](https://mindmajix.com/sccm-inventory)).
- **Gaps**: Devices without SCCM clients miss software inventory.
- **Firewall**: Agent-based, no scanning; requires SQL access (port 1433) for MID server to SCCM database.

### 2. Cloud Discovery Covered Devices
- **Coverage**: Azure resources (e.g., VMs) discovered via API.
- **Software Data**: Relies on SCCM clients for software inventory; no direct software data from Cloud Discovery.
- **Gaps**: Non-SCCM Azure VMs, especially Linux/Mac, lack software inventory.
- **Firewall**: API-based, requires outbound HTTPS from MID server; no scanning.

### 3. On-Premises Devices
- **Coverage**: SCCM-covered devices report software inventory; others require ServiceNow Discovery or ACC Agent.
- **Software Data**: Non-SCCM devices need alternative methods for vendor-agnostic software data.
- **Gaps**: Non-SCCM on-prem devices, especially non-Windows, are blind spots.
- **Firewall**: ServiceNow Discovery requires firewall rules (e.g., SSH:22, WMI:135) from MID to target subnets.

## Potential Gaps
- **Non-SCCM Devices**: Any device without SCCM client (Windows, Linux, Mac) lacks software inventory.
- **Non-Windows Devices**: Linux/Mac without SCCM clients need ACC Agent or Discovery.
- **SaaS Applications**: SAM may miss cloud-based software not installed locally.
- **Software Detection**: SCCM may miss niche or custom software not registered in standard locations.

## Firewall and Scanning Implications
- **SCCM/ACC**: Agent-based, devices phone home; no scanning or firewall changes needed beyond MID server access to SCCM database or ServiceNow instance.
- **Azure Cloud Discovery**: API-based, no scanning; requires MID server outbound internet access.
- **ServiceNow Discovery**: Active scanning for non-SCCM on-prem devices requires firewall rules from MID server source IPs to destination subnets, potentially numerous depending on network segmentation.

## Recommendations
1. **Optimize SCCM**: Ensure SCCM clients are installed on all Windows, Linux, and Mac devices to maximize software inventory coverage ([Mac Management](https://www.anoopcnair.com/sccm-mac-management-take-decision-in-10-mins/)).
2. **Gap Analysis**: Identify non-SCCM devices via network scans or manual audits; assess need for ACC Agent or ServiceNow Discovery.
3. **ACC Agent**: Deploy for non-SCCM Linux/Mac devices to collect software inventory without scanning ([Agent Client Collector](https://www.servicenow.com/community/itom-articles/agent-client-collector-acc-for-discovery-and-asset-management/ta-p/2314970)).
4. **ServiceNow Discovery**: Use for on-prem non-SCCM devices if ACC is impractical; plan firewall rules carefully.
5. **SaaS Integration**: Explore SAM integrations for SaaS apps if relevant.

## Blind Spots
- **Unmanaged Devices**: Rogue or unmanaged devices not in SCCM or Azure.
- **Dynamic Environments**: Rapidly provisioned Azure resources without SCCM clients.
- **Complex Software**: Software not detected by SCCM due to non-standard installation.

## Conclusion
Optimizing SCCM and Azure Cloud Discovery provides robust SAM coverage for devices with SCCM clients, capturing multi-vendor software data. Gaps in non-SCCM devices can be addressed with ACC Agent or ServiceNow Discovery, with careful firewall planning for the latter. A gap analysis will ensure comprehensive software asset management.



---

**Software Vendor Coverage**
SCCM integration in ServiceNow likely captures software from multiple vendors, not just Microsoft, for Windows, Linux, and Mac devices with SCCM clients installed ([SCCM Inventory](https://mindmajix.com/sccm-inventory)). Azure Cloud Discovery identifies Azure resources but relies on SCCM for software data, ensuring vendor-agnostic coverage if clients are present.

**ACC Agent for Macs/Linux**
If SCCM clients are installed on Macs and Linux machines, software inventory is likely covered, reducing the need for the ACC Agent ([Mac Management](https://www.anoopcnair.com/sccm-mac-management-take-decision-in-10-mins/)). The ACC Agent is beneficial for non-SCCM devices, providing agent-based inventory collection without scanning.

**Firewall and Scanning Needs**
SCCM and ACC Agent are agent-based, requiring no scanning or additional firewall rules beyond MID server access to SCCM databases (port 1433) or ServiceNow ([ServiceNow Elite](https://www.servicenowelite.com/blog/2019/8/4/microsoft-sccm-integration)). Azure Cloud Discovery uses APIs, needing only outbound HTTPS. ServiceNow Discovery for on-prem non-SCCM devices requires firewall rules (e.g., SSH:22, WMI:135) from MID server IPs to target subnets, potentially numerous based on network complexity ([ServiceNow Discovery](https://www.servicenow.com/community/itom-articles/discovery-101/ta-p/2314968)).

**Compelling Message**
Focus on optimizing SCCM and Azure Cloud Discovery for SAM by ensuring SCCM clients are installed on all devices. Perform a gap analysis to identify non-SCCM devices, using ACC Agent or ServiceNow Discovery as needed, with minimal firewall adjustments for agent-based methods.

**Potential Blind Spots**
- Devices without SCCM clients, especially non-Windows, are likely missed.
- SaaS applications or cloud-based software not installed locally may be overlooked.
- Unmanaged or rogue devices not in SCCM or Azure could be blind spots.
- Niche software not detected by SCCM due to non-standard installation.

**Key Citations**
- [What's New in Yokohama Release for Software Asset Management](https://www.servicenow.com/community/sam-blog/what-s-new-in-yokohama-release-for-software-asset-management/ba-p/3152533)
- [Microsoft SCCM Integration with ServiceNow](https://www.servicenowelite.com/blog/2019/8/4/microsoft-sccm-integration)
- [Software Inventory in Configuration Manager](https://learn.microsoft.com/en-us/intune/configmgr/core/clients/manage/inventory/introduction-to-software-inventory)
- [Managing Linux Computers Using System Center 2012 R2 Configuration Manager](https://www.prajwaldesai.com/managing-linux-computers-using-system-center-2012-r2-configuration-manager/)
- [How to Prepare Software Inventories of Mac Computers in SCCM](https://www.parallels.com/blogs/manage-mac/mac-software-inventory/)
- [Microsoft SCCM Software and Hardware Inventory](https://mindmajix.com/sccm-inventory)
- [SCCM Mac Management Decision Guide](https://www.anoopcnair.com/sccm-mac-management-take-decision-in-10-mins/)
- [ServiceNow Discovery 101 Overview](https://www.servicenow.com/community/itom-articles/discovery-101/ta-p/2314968)
- [Agent Client Collector for Discovery and Asset Management](https://www.servicenow.com/community/itom-articles/agent-client-collector-acc-for-discovery-and-asset-management/ta-p/2314970)

- 


# Research SAM

ServiceNow SAM Credential Requirements for Software License Tracking
Overview
ServiceNow Software Asset Management (SAM) tracks software license consumption by integrating with the Discovery module and vendor APIs. This requires specific credentials to access on-premises systems and cloud-based services, ensuring accurate and granular license data. Below is a detailed analysis of credential requirements, focusing on Microsoft, Adobe, Oracle, Citrix, and other common vendors, as of May 14, 2025.
Credential Requirements for On-Premises Systems
Windows Servers

Credential Type: Administrative credentials (domain admin or local admin).
Purpose: Allows ServiceNow Discovery to query servers for installed software and license usage data, such as installation details and usage metrics.
Necessity: Administrative access is typically required to access the Windows registry and system files where software details are stored.
Example: A domain admin account with permissions to query all Windows servers in the environment.

Linux/Unix Servers

Credential Type: SSH credentials with sufficient permissions.
Purpose: Enables Discovery to access Linux/Unix systems to inventory installed software and gather license information.
Necessity: Permissions must allow access to software installation directories and configuration files.
Example: An SSH user account with read access to system directories.

Databases (e.g., Oracle, MSSQL)

Credential Type: OS-level credentials for basic discovery; database-specific credentials for detailed metrics.
Purpose: OS credentials allow discovery of database software; database credentials provide access to usage metrics, such as active sessions or historical data.
Necessity: For Oracle, access to tables like dba_hist_active_sess_history requires database credentials to track license consumption accurately.
Example: An Oracle database user with read access to licensing-related tables.

Network Devices

Credential Type: SNMP credentials or network management credentials.
Purpose: Used to discover devices hosting software licenses, such as network appliances with embedded software.
Necessity: SNMP community strings or equivalent are needed for device access.
Example: An SNMP v2/v3 community string with read-only access.

Credential Requirements for SaaS and Cloud-Based Vendors
Adobe

Credential Type: OAuth credentials (client ID, client secret, organization ID).
Purpose: Connects ServiceNow SAM to Adobe Cloud via API to retrieve subscription and usage data.
Necessity: OAuth credentials are provided by Adobe's admin console, enabling direct integration without accessing end-user systems.
Example: Configured in ServiceNow under the Adobe SaaS connector, using credentials from Adobe's admin portal (Adobe Integration Profile Configuration).

Citrix

Credential Type: Server credentials for on-premises; API credentials for cloud-based services.
Purpose: On-premises Citrix deployments require OS credentials to discover software on servers; cloud services may use API keys or integration profiles.
Necessity: Depends on deployment model—on-premises needs server access, while cloud requires vendor-provided authentication.
Example: Windows admin credentials for on-premises Citrix XenApp servers; API keys for Citrix Cloud.

Oracle (Cloud-Based)

Credential Type: API keys or OAuth credentials for cloud services; OS/database credentials for on-premises.
Purpose: Cloud-based Oracle services use API integrations; on-premises Oracle software requires server and database access.
Necessity: API credentials enable direct data retrieval from Oracle's cloud platform; on-premises needs elevated access for detailed metrics.
Example: Oracle Identity Cloud Service uses OAuth for SSO integration (Oracle Identity Cloud Service SSO).

Other Vendors (e.g., SAP, VMware, Red Hat)

Credential Type: Varies by deployment—OS credentials for on-premises; API credentials for cloud.
Purpose: Tracks software licenses on servers or through vendor APIs.
Necessity: On-premises requires server access; cloud services use vendor-specific authentication.
Example: SAP may require ABAP program credentials for on-premises discovery; VMware may use vCenter API credentials.

Why Privileged Credentials Are Needed

Granular License Data: SAM requires detailed data (e.g., installation paths, usage metrics, active sessions) to track license consumption accurately, often necessitating administrative or database-level access.
Discovery Process: ServiceNow Discovery uses probes (e.g., Shazzam, classification probes) that need credentials to access systems and retrieve software details (ServiceNow Discovery Configuration).
Vendor-Specific Metrics: Vendors like Oracle require access to specific data points (e.g., database tables) for compliance, which demands privileged credentials (Oracle Database Credentials).

Credential Management and Security

Centralized Management: Store credentials securely in ServiceNow's Discovery > Credentials module, testing them with the Test Credential option.
Security Considerations: Limit credential scope, use read-only accounts where possible, and monitor usage to prevent unauthorized access.
Challenges: Managing credentials across dispersed environments can be complex, requiring coordination with IT and database teams.

Tables for Reference



System/Vendor
Credential Type
Purpose
Example



Windows Servers
Domain/Local Admin
Query software installations
Domain admin account


Linux Servers
SSH
Inventory software
SSH user with read access


Oracle Database
Database Credentials
Access license metrics
User with dba_hist_active_sess_history access


Adobe
OAuth (Client ID, Secret)
API integration
Adobe admin console credentials


Citrix
Server/API Credentials
On-premises or cloud discovery
Windows admin or Citrix Cloud API key


Network Devices
SNMP
Discover device software
SNMP v3 community string





Aspect
Details



Necessity
Privileged credentials ensure granular data for compliance and optimization.


Security
Use read-only accounts, encrypt credentials, and limit scope.


Management
Centralized in ServiceNow; requires coordination with IT teams.


Conclusion
Privileged credentials are essential for ServiceNow SAM to track software license consumption, particularly for on-premises systems like Windows, Linux, and Oracle databases. SaaS vendors like Adobe and cloud-based Citrix or Oracle services use API integrations with OAuth or API keys. Ensuring proper credential configuration is critical for accurate license tracking and compliance.



### Key Points
- Privileged credentials are likely needed for on-premises software license tracking.
- Windows servers may require admin credentials for detailed data.
- Adobe, Oracle, Citrix need specific credentials, often API-based for SaaS.
- Granular license details typically demand elevated access.

### On-Premises Credentials
For Windows servers, administrative credentials (domain or local admin) are likely needed to access software installation and license usage details. Linux servers require SSH credentials with sufficient permissions. Oracle databases may need database-specific credentials for detailed metrics.

### SaaS and Vendor-Specific Credentials
Adobe uses OAuth credentials (client ID, secret, organization ID) for API integration. Citrix may require server credentials for on-premises or API credentials for cloud. Oracle on-premises needs OS and database credentials; cloud may use API keys.

### Granular License Details
Privileged credentials are generally required for granular license data, ensuring ServiceNow SAM can track consumption accurately across vendors.

---


# ServiceNow SAM Credential Requirements for Software License Tracking

## Overview
ServiceNow Software Asset Management (SAM) tracks software license consumption by integrating with the Discovery module and vendor APIs. This requires specific credentials to access on-premises systems and cloud-based services, ensuring accurate and granular license data. Below is a detailed analysis of credential requirements, focusing on Microsoft, Adobe, Oracle, Citrix, and other common vendors, as of May 14, 2025.

## Credential Requirements for On-Premises Systems

### Windows Servers
- **Credential Type**: Administrative credentials (domain admin or local admin).
- **Purpose**: Allows ServiceNow Discovery to query servers for installed software and license usage data, such as installation details and usage metrics.
- **Necessity**: Administrative access is typically required to access the Windows registry and system files where software details are stored.
- **Example**: A domain admin account with permissions to query all Windows servers in the environment.

### Linux/Unix Servers
- **Credential Type**: SSH credentials with sufficient permissions.
- **Purpose**: Enables Discovery to access Linux/Unix systems to inventory installed software and gather license information.
- **Necessity**: Permissions must allow access to software installation directories and configuration files.
- **Example**: An SSH user account with read access to system directories.

### Databases (e.g., Oracle, MSSQL)
- **Credential Type**: OS-level credentials for basic discovery; database-specific credentials for detailed metrics.
- **Purpose**: OS credentials allow discovery of database software; database credentials provide access to usage metrics, such as active sessions or historical data.
- **Necessity**: For Oracle, access to tables like `dba_hist_active_sess_history` requires database credentials to track license consumption accurately.
- **Example**: An Oracle database user with read access to licensing-related tables.

### Network Devices
- **Credential Type**: SNMP credentials or network management credentials.
- **Purpose**: Used to discover devices hosting software licenses, such as network appliances with embedded software.
- **Necessity**: SNMP community strings or equivalent are needed for device access.
- **Example**: An SNMP v2/v3 community string with read-only access.

## Credential Requirements for SaaS and Cloud-Based Vendors

### Adobe
- **Credential Type**: OAuth credentials (client ID, client secret, organization ID).
- **Purpose**: Connects ServiceNow SAM to Adobe Cloud via API to retrieve subscription and usage data.
- **Necessity**: OAuth credentials are provided by Adobe's admin console, enabling direct integration without accessing end-user systems.
- **Example**: Configured in ServiceNow under the Adobe SaaS connector, using credentials from Adobe's admin portal ([Adobe Integration Profile Configuration](https://support.servicenow.com/kb?id=kb_article_view&sysparm_article=KB1001915)).

### Citrix
- **Credential Type**: Server credentials for on-premises; API credentials for cloud-based services.
- **Purpose**: On-premises Citrix deployments require OS credentials to discover software on servers; cloud services may use API keys or integration profiles.
- **Necessity**: Depends on deployment model—on-premises needs server access, while cloud requires vendor-provided authentication.
- **Example**: Windows admin credentials for on-premises Citrix XenApp servers; API keys for Citrix Cloud.

### Oracle (Cloud-Based)
- **Credential Type**: API keys or OAuth credentials for cloud services; OS/database credentials for on-premises.
- **Purpose**: Cloud-based Oracle services use API integrations; on-premises Oracle software requires server and database access.
- **Necessity**: API credentials enable direct data retrieval from Oracle's cloud platform; on-premises needs elevated access for detailed metrics.
- **Example**: Oracle Identity Cloud Service uses OAuth for SSO integration ([Oracle Identity Cloud Service SSO](https://docs.oracle.com/en/cloud/paas/identity-cloud/idcsc/servicenow.html)).

### Other Vendors (e.g., SAP, VMware, Red Hat)
- **Credential Type**: Varies by deployment—OS credentials for on-premises; API credentials for cloud.
- **Purpose**: Tracks software licenses on servers or through vendor APIs.
- **Necessity**: On-premises requires server access; cloud services use vendor-specific authentication.
- **Example**: SAP may require ABAP program credentials for on-premises discovery; VMware may use vCenter API credentials.

## Why Privileged Credentials Are Needed
- **Granular License Data**: SAM requires detailed data (e.g., installation paths, usage metrics, active sessions) to track license consumption accurately, often necessitating administrative or database-level access.
- **Discovery Process**: ServiceNow Discovery uses probes (e.g., Shazzam, classification probes) that need credentials to access systems and retrieve software details ([ServiceNow Discovery Configuration](https://servicenowwithrunjay.com/servicenow-discovery-step-by-step-configuration/)).
- **Vendor-Specific Metrics**: Vendors like Oracle require access to specific data points (e.g., database tables) for compliance, which demands privileged credentials ([Oracle Database Credentials](https://www.servicenow.com/community/itom-forum/oracle-database-applicative-credentials/m-p/1020786)).

## Credential Management and Security
- **Centralized Management**: Store credentials securely in ServiceNow's Discovery > Credentials module, testing them with the Test Credential option.
- **Security Considerations**: Limit credential scope, use read-only accounts where possible, and monitor usage to prevent unauthorized access.
- **Challenges**: Managing credentials across dispersed environments can be complex, requiring coordination with IT and database teams.

## Tables for Reference

| **System/Vendor** | **Credential Type** | **Purpose** | **Example** |
|-------------------|---------------------|-------------|-------------|
| Windows Servers | Domain/Local Admin | Query software installations | Domain admin account |
| Linux Servers | SSH | Inventory software | SSH user with read access |
| Oracle Database | Database Credentials | Access license metrics | User with `dba_hist_active_sess_history` access |
| Adobe | OAuth (Client ID, Secret) | API integration | Adobe admin console credentials |
| Citrix | Server/API Credentials | On-premises or cloud discovery | Windows admin or Citrix Cloud API key |
| Network Devices | SNMP | Discover device software | SNMP v3 community string |

| **Aspect** | **Details** |
|------------|-------------|
| **Necessity** | Privileged credentials ensure granular data for compliance and optimization. |
| **Security** | Use read-only accounts, encrypt credentials, and limit scope. |
| **Management** | Centralized in ServiceNow; requires coordination with IT teams. |

## Conclusion
Privileged credentials are essential for ServiceNow SAM to track software license consumption, particularly for on-premises systems like Windows, Linux, and Oracle databases. SaaS vendors like Adobe and cloud-based Citrix or Oracle services use API integrations with OAuth or API keys. Ensuring proper credential configuration is critical for accurate license tracking and compliance.



### Key Citations
- [ServiceNow Discovery Step-by-Step Configuration](https://servicenowwithrunjay.com/servicenow-discovery-step-by-step-configuration/)
- [Credentials Issues in Discovery - ServiceNow Community](https://www.servicenow.com/community/itom-forum/credentials-issues-in-discovery/m-p/2775134)
- [ServiceNow Discovery Credentials Ultimate Guide](https://infocenter.io/servicenow-discovery-credentials/)
- [Adobe Integration Profile Configuration Step-By-Step](https://support.servicenow.com/kb?id=kb_article_view&sysparm_article=KB1001915)
- [Oracle Database Applicative Credentials - ServiceNow Community](https://www.servicenow.com/community/itom-forum/oracle-database-applicative-credentials/m-p/1020786)
- [ServiceNow SAM Pro - Licensing Data Solutions](https://www.licensingdatasolutions.com/sam-tooling/servicenow)
- [Oracle Identity Cloud Service SSO Configuration](https://docs.oracle.com/en/cloud/paas/identity-cloud/idcsc/servicenow.html)
