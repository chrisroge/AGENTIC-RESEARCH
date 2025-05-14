# Key points
- SCCM can provide signfiicant SAM coverage with minimal disruption
- Cloud Discovery may provide additional coverage with minimal disruption
- Discovery beyond SCCM and Cloud Discovery will require a significant investiment in time and complexity with MID servers requiring line of sight across all multiple firewall layers, large coordination with infrasructure and security teams require and independent impact statements are required from each tower
- Ongoing operational debt will include management of license entitlements, reporting, administrative actions related to licensing etc.

## Focus 
Focus on optimizing SCCM and Azure Cloud Discovery for SAM by ensuring SCCM clients are installed on all devices. Perform a gap analysis to identify non-SCCM devices, using ACC Agent or ServiceNow Discovery as needed, with minimal firewall adjustments for agent-based methods.

## ServiceNow SAM Discovery Capabilities in Yokohama (Software Inventory & Reconciliation)
ServiceNow SAM (Professional) is designed to automatically discover installed software across your environment and translate that into meaningful license positions. In grounded terms, this means the platform can identify software installed on servers and computers (e.g. Windows applications, Linux packages) and normalize those findings to a standardized catalog of software products. For example, if a VM has Microsoft SQL Server 2019 installed, ServiceNow will create a record of that installation, normalize it to the standardized product name/version (a “Discovery Model”), and then count it against your SQL Server license entitlements
servicenow.com
. The Yokohama release continues to use ServiceNow’s Software Library for normalization, ensuring discovered titles (from various sources) are matched to known publishers, versions, and editions. In practical terms, software inventory data can come from multiple discovery sources. ServiceNow SAM supports using its own Discovery tool or third-party inventory tools like Microsoft SCCM to gather installed software information
servicenow.com
servicenow.com
. Once the data is in the platform, SAM’s normalization engine cleans up naming (creating “Software Discovery Models”) and maps them to Software Models which have licensing metrics attached. A reconciliation engine then compares the total installations (consumption) to the purchased license entitlements for each software product, determining compliance, under-use, or overspending. The Yokohama release introduced enhancements like Now Assist (an AI helper) and guided setup for SAM, but at its core the inventory and license reconciliation process remains reliant on accurate discovery of software installations
servicenow.com
servicenow.com
. What can ServiceNow SAM discovery achieve? In real-world terms, it can provide a single pane of glass for all software installed in your environment (on desktops, servers, cloud VMs, etc.), who/what it’s installed on, and whether each installation is covered by a license. It captures details like the software name, version, publisher, install date, and usage information (if available), and then uses that to:
Normalize software titles – e.g. recognizing that “MS Office 16.0” and “Microsoft Office 2016” are the same product, via the SAM Content Library. This automatic normalization ensures consistent naming
servicenow.com
 and is essential for accurate license counting.
Identify license requirements – e.g. flagging that SQL Server installations require a SQL license, or that Adobe Acrobat installs count against your Adobe license pool.
Perform compliance reconciliation – by comparing normalized installation counts to your license entitlements, SAM can tell you if you are under-licensed (risking compliance issues) or over-licensed (opportunities to save cost).
Optimize usage – if usage data is collected, SAM can identify unused installations (e.g. software installed but not used in 90 days) for potential removal (reclamation). For example, with certain data sources, ServiceNow can track last used dates to reclaim unused licenses. Usage tracking typically requires either SCCM metering or the ServiceNow client agent, which we discuss later.
In summary, as of Yokohama, ServiceNow SAM’s discovery capabilities can bring in software installation data from both agentless scans and agent-based sources, create an accurate inventory of software across on-prem and cloud infrastructure, and feed it into a license management engine. Next, we’ll examine how you can gather that software data – comparing SCCM vs. Azure integration vs. ServiceNow’s own Discovery (MID Server) vs. agent-based collection – and the practical implications of each approach.

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

## Deep focus on SAM
Understood. I’ll prepare a deep dive into ServiceNow Software Asset Management capabilities as of the Yokohama release, with a focus on software discovery. This will include an evaluation of discovery methods (SCCM, Azure, and ServiceNow-native options), MID Server requirements including firewall and line-of-sight considerations, and a complexity-to-capability matrix. I’ll also explore the trade-offs between agentless scanning, MID-based probes, and device-initiated reporting to highlight potential disruption vs. value.


# ServiceNow SAM Software Discovery (Yokohama Release) – Capabilities, Methods, and Trade‑offs

## Introduction

ServiceNow’s Software Asset Management (SAM) module in the Yokohama release provides robust capabilities for discovering installed software and reconciling it against licenses. In a real-world scenario (such as the one described, with Microsoft SCCM 2016 feeding data eight times daily and Azure cloud resources integrated via the ServiceNow Cloud Management plugin), it’s important to understand what SAM discovery can achieve and how different discovery approaches compare. This report focuses on **software discovery** – specifically how ServiceNow SAM gathers software inventory data for on-premises and Azure-based systems – and analyzes the methods available (SCCM integration, Azure native discovery, MID Server agentless discovery, and agent-based approaches). We will describe these capabilities in practical terms, compare their complexity and benefits, and recommend how to maximize software visibility while minimizing network “noise.” Hardware inventory, SaaS application tracking, and cloud cost features are outside our scope, keeping the focus on installed software discovery and license reconciliation.

## ServiceNow SAM Discovery Capabilities in Yokohama (Software Inventory & Reconciliation)

ServiceNow SAM (Professional) is designed to automatically **discover installed software** across your environment and translate that into meaningful license positions. In grounded terms, this means the platform can identify software installed on servers and computers (e.g. Windows applications, Linux packages) and normalize those findings to a standardized catalog of software products. For example, if a VM has **Microsoft SQL Server 2019** installed, ServiceNow will create a record of that installation, normalize it to the standardized product name/version (a “Discovery Model”), and then count it against your SQL Server license entitlements. The Yokohama release continues to use ServiceNow’s Software Library for normalization, ensuring discovered titles (from various sources) are matched to known publishers, versions, and editions.

In practical terms, **software inventory data** can come from multiple discovery sources. ServiceNow SAM supports using its **own Discovery** tool or **third-party inventory tools** like Microsoft SCCM to gather installed software information. Once the data is in the platform, SAM’s normalization engine cleans up naming (creating “Software Discovery Models”) and maps them to **Software Models** which have licensing metrics attached. A reconciliation engine then compares the total installations (consumption) to the purchased license entitlements for each software product, determining compliance, under-use, or overspending. The Yokohama release introduced enhancements like Now Assist (an AI helper) and guided setup for SAM, but at its core the inventory and license reconciliation process remains reliant on accurate discovery of software installations.

**What can ServiceNow SAM discovery achieve?** In real-world terms, it can provide a single pane of glass for all software installed in your environment (on desktops, servers, cloud VMs, etc.), who/what it’s installed on, and whether each installation is covered by a license. It captures details like the software name, version, publisher, install date, and usage information (if available), and then uses that to:

* **Normalize software titles** – e.g. recognizing that “MS Office 16.0” and “Microsoft Office 2016” are the same product, via the SAM Content Library. This automatic normalization ensures consistent naming and is essential for accurate license counting.
* **Identify license requirements** – e.g. flagging that SQL Server installations require a SQL license, or that Adobe Acrobat installs count against your Adobe license pool.
* **Perform compliance reconciliation** – by comparing normalized installation counts to your license entitlements, SAM can tell you if you are under-licensed (risking compliance issues) or over-licensed (opportunities to save cost).
* **Optimize usage** – if usage data is collected, SAM can identify unused installations (e.g. software installed but not used in 90 days) for potential removal (reclamation). *For example, with certain data sources, ServiceNow can track last used dates to reclaim unused licenses.* Usage tracking typically requires either SCCM metering or the ServiceNow client agent, which we discuss later.

In summary, as of Yokohama, ServiceNow SAM’s discovery capabilities can bring in software installation data from both agentless scans and agent-based sources, create an accurate inventory of software across on-prem and cloud infrastructure, and feed it into a license management engine. Next, we’ll examine **how** you can gather that software data – comparing SCCM vs. Azure integration vs. ServiceNow’s own Discovery (MID Server) vs. agent-based collection – and the practical implications of each approach.

## Discovery Methods for Software Inventory in ServiceNow SAM

ServiceNow supports multiple discovery methods to populate the software inventory needed by SAM. Each method has different requirements and trade-offs in terms of complexity, coverage, and network impact. The major approaches are:

* **Microsoft SCCM Integration (Service Graph Connector for SCCM)** – leveraging the existing SCCM 2016 deployment and its agent data.
* **Azure Native Discovery (Cloud Management integration)** – using Azure’s APIs (via the ServiceNow Cloud plugin) to pull cloud resource data.
* **ServiceNow Horizontal Discovery (MID Server with probes/patterns)** – ServiceNow’s agentless discovery scanning for software on targets (on-prem or cloud VMs).
* **Agent-Based Client Discovery (ServiceNow Agent Client Collector)** – deploying ServiceNow’s own agent to push software data from devices (an emerging method for endpoints).

Let’s discuss each method in real-world terms, focusing on how they discover software, what they require to set up, and their pros and cons.

### 1. SCCM Integration (Agent-Based Inventory via Microsoft SCCM)

**How it works:** If you have Microsoft System Center Configuration Manager (SCCM) already collecting inventory from your Windows devices (which is the case here, with SCCM 2016 polling clients 8x daily), ServiceNow can ingest that data through a **Service Graph Connector**. SCCM’s client agent on each Windows machine gathers installed software entries from the OS (usually from Add/Remove Programs for Windows) and stores it in the SCCM database. The ServiceNow connector then pulls this data on a scheduled basis (eight times per day in the scenario, which is quite frequent and near real-time). Each import maps to ServiceNow’s CMDB and SAM tables, creating or updating the computer CI records and the associated **Installed Software** records for each application found on those computers. This gives ServiceNow SAM a view of all software that SCCM knows about.

**Capabilities:** The SCCM integration is quite comprehensive for Windows environments. The latest SCCM connector is *not limited to specific software titles* – it will bring in the full list of installed software from each device, as recorded by the SCCM agent. In other words, if SCCM’s inventory shows 50 applications installed on a given server, all 50 will be imported into ServiceNow (with details like software name, version, install location, etc.). The quality and depth of this data depend on SCCM’s configuration (for example, SCCM can be set to collect additional attributes or usage data for certain apps). One advantage of SCCM as a source is that it can also provide user-to-device relationships and usage metrics; SCCM knows which user is the primary user of a PC and can meter application usage. This means ServiceNow can leverage SCCM data to link installations to users (useful for per-user licensing) and see how often an application is used if SCCM’s Software Metering is enabled. All imported data is then **normalized by ServiceNow SAM** using the SAM content library, just as with any discovery source, so the same reconciliation process applies.

**Complexity and Deployment:** Integrating SCCM is relatively straightforward since the heavy lifting of discovery is already done by the SCCM agents. You need to install/configure the SCCM connector (usually a scoped app from the ServiceNow Store), provide it access to the SCCM database or API, and map fields. In Yokohama (and recent releases), this is typically a **Service Graph Connector** which uses the Identification and Reconciliation Engine (IRE) to merge data properly. The SCCM connector runs as scheduled jobs, so you’ll schedule data pulls (in this case, eight times a day to align with SCCM’s frequent updates). **No additional network scanning** is required by ServiceNow – it’s pulling from SCCM’s central source. This means **minimal incremental complexity** if SCCM is already in place: no new agents to deploy (it reuses the SCCM client on machines) and limited infrastructure changes. Do ensure the integration is tuned to avoid data overlap; for example, if you previously used ServiceNow Discovery on the same machines, you’ll want to reconcile or clean up any duplicate software records so that SCCM becomes the primary source going forward.

**Pros:** Using SCCM data has several advantages:

* *Leverages existing tooling:* It utilizes the SCCM agent already installed on devices, which means you don’t introduce new processes on endpoints.
* *Comprehensive inventory:* SCCM is designed for desktop/server management and pulls a detailed list of installed programs (down to minor versions). It’s proven and reliable for Windows software discovery.
* *User context and frequency:* SCCM can provide the username of the primary user of the device and possibly software usage info, which pure network scanning cannot. Also, since the SCCM agent checks in regularly, you can get updates multiple times per day without having to “ping” every device from the network each time. The data is device-initiated (collected by the agent and sent to SCCM server), which reduces network noise.
* *Minimal network impact:* All communication is between SCCM and ServiceNow (usually via an API or database query over the network), not a broad scan of all endpoints by ServiceNow. This avoids the perception of a “scan” from the ServiceNow side – the heavy work is done by SCCM behind the scenes, and ServiceNow just imports the results.

**Cons / Trade-offs:**

* *Windows-centric:* SCCM primarily manages Windows devices. If you have Linux or Mac systems in Azure not covered by SCCM, those won’t be discovered by this method. (SCCM 2016 had limited support for Linux/Mac; many orgs use other tools for those.)
* *Dependency on SCCM data quality:* ServiceNow will only know what SCCM reports. If SCCM’s inventory misses a software (for example, if an app doesn’t properly register in Add/Remove Programs or the SCCM agent is unhealthy on a client), then SAM will miss it too. Essentially, the coverage and accuracy are only as good as SCCM’s.
* *SCCM Infrastructure:* You must maintain the SCCM server infrastructure and keep the agents healthy. This is not an issue for many enterprises, but it’s an extra layer – if SCCM goes down or is slow, your SAM data might lag.
* *Latency and Data Mapping:* While eight imports per day is quite frequent, it’s not truly real-time. There could be a few hours’ delay between a software install and it showing up in ServiceNow via SCCM. Also, initial setup of the connector requires mapping SCCM data fields to ServiceNow’s tables (which is mostly handled by the out-of-box connector content, but it’s a step to be done carefully).

**Use Case Fit:** Given the scenario (primarily Azure-based, small on-prem footprint), if most servers/VMs are running Windows with SCCM agents, this integration covers a large portion of the software inventory without additional discovery overhead. It is ideal for **end-user computers (EUC) and Windows servers** where the SCCM agent is present. In fact, a ServiceNow employee notes that **SCCM (with its agent) and ServiceNow Discovery (agentless) can “co-exist well,” covering different needs – SCCM excels for user devices, while agentless discovery is well-suited for servers and network gear**. We’ll discuss those complementary agentless methods next.

### 2. Azure Native Integration (Cloud Discovery via Azure APIs)

**How it works:** ServiceNow provides a Cloud Discovery capability (through the **Cloud Management plugin or Service Graph connectors for cloud** providers) which integrates with Azure to retrieve information about cloud resources. In this user’s context, Azure subscriptions are already integrated, meaning ServiceNow can pull in Azure **VM instances, their configurations, and metadata** (like OS type, IP addresses, VM size, tags, etc.) as CIs in the CMDB. Azure integration typically works via API calls (using an Azure service principal) rather than network scanning. A ServiceNow MID Server is configured to authenticate to Azure and call Azure Resource Management APIs to list resources in the subscriptions. This method is **agentless and does not require credentials on each VM** – it’s reading the cloud management plane, not logging into the OS.

**Capabilities:** Out of the box, Azure cloud discovery will enumerate VMs (along with other Azure components like storage accounts, databases, networks if configured) and populate them in CMDB tables (e.g., Azure Virtual Machine CI records). It provides **infrastructure inventory** – for example, it can tell you that a VM named “AppServer1” exists in Azure, is a Windows Server 2019 machine of size Standard\_D2\_v2, located in region X, with IP address Y. This ensures all Azure-hosted VMs are accounted for in ServiceNow. However, **Azure’s API does not divulge the list of installed software on a VM** by default. The cloud discovery is focused on resource attributes, not inside-OS details. So, by itself, Azure integration **cannot directly populate the “installed software” data for SAM**. It tells us *what* VMs exist (preventing blind spots), but not *what software* those VMs have beyond the OS name.

There are some Azure capabilities that could theoretically help with software inventory (for example, Azure Automation’s **Inventory** or **Azure Arc** which can list installed applications if an agent is installed on the VM, or Log Analytics data). But out-of-the-box ServiceNow Cloud Discovery doesn’t automatically use those – it would require custom integration. In Yokohama’s standard features, **software discovery on cloud VMs would still rely on either agentless scanning or another agent, not just the Azure connector**. So the Azure native integration’s “capability” for SAM is indirect: it makes sure all cloud VMs are discovered as configuration items, which you can then target with other discovery methods (like running a ServiceNow Discovery pattern, or ensuring an SCCM/ACC agent is on them).

**Complexity and Setup:** Azure integration is typically easier to set up compared to full infrastructure discovery. You need to create a service principal in Azure with read permissions to the resources, install a MID Server (which can be on-prem or in Azure) that can reach Azure’s public endpoints, and configure credentials in ServiceNow. There’s no need to configure IP ranges or credentials for each server. **Network-wise**, the MID Server just needs outbound HTTPS (port 443) access to Azure’s APIs. You do not need to open any inbound ports to your VMs for this phase. This makes it low-impact in terms of firewall changes. Once configured, discovery jobs will periodically call Azure and update the CMDB records.

**Pros:**

* *Covers Cloud Sprawl:* Ensures every VM in every connected Azure subscription is known to ServiceNow, even if those VMs aren’t otherwise reachable. This is critical in cloud environments where instances can be spun up quickly; API discovery catches new VMs without needing a network scan.
* *No Credentials on VMs:* You don’t need Windows or Linux login credentials to get the basic inventory. This simplifies security since you’re not storing admin creds for each VM just to discover it.
* *Minimal Network Noise:* All communication is a few API calls over the internet (or via your network proxy) on port 443. It doesn’t probe each IP or generate traffic *to* each VM at this stage. From a network perspective, it’s just the MID server talking to Azure cloud endpoints, which is very standard web traffic.
* *Foundation for Deeper Discovery:* By having the VM CI records in place (with info like IP and OS), ServiceNow can later use that information to drive deeper discovery. For example, if you choose to run a **pattern-based discovery** on those Azure VMs, ServiceNow already knows how to reach them (IP, and perhaps which MID server to use). Cloud discovery can thus be phase 1, and detailed software discovery phase 2.

**Cons / Limitations:**

* *No Software Details by Default:* As noted, this method alone doesn’t fulfill the software inventory need. If one were to rely *only* on Azure integration, SAM would see the VMs and their OS, but you’d have zero records of specific software installed inside those VMs. This is insufficient for license management (you’d know you have a Windows VM, but not if SQL or Oracle or Adobe is installed on it).
* *Requires Additional Steps for SAM:* You will have to complement Azure API discovery with either SCCM (if those VMs have SCCM agents) or ServiceNow’s own discovery/agent to actually get the software installs. Azure integration is necessary but not sufficient for SAM in a cloud-first environment.
* *Permission and Data Gaps:* Azure’s API might not reveal certain things (like license keys or detailed configurations) that an OS-level discovery would. Also, if VMs are using ephemeral OS disks or other edge cases, the data might not be complete (though for the most part it is accurate for basic attributes).
* *Azure-Only:* This covers Azure VMs. If you have multi-cloud (AWS, GCP), you’d need similar integrations for each. (The question is Azure-specific, but in general, each cloud needs its own integration configuration.)

**Use Case Fit:** In our scenario, with primarily Azure-based infrastructure, the Azure Cloud plugin integration is a great starting point. It ensures your ServiceNow CMDB isn’t missing any Azure VMs. The user already has this working (“Discovery is currently functional for Azure tenants”). This means they likely see all their Azure VMs in the CMDB. The next step is leveraging that for SAM – either by making sure the SCCM agent is deployed to those VMs or using ServiceNow Discovery via a MID Server located in Azure (we cover that next). In summary, Azure native discovery gives **breadth** (visibility of all resources) with **low complexity**, but it must be paired with another approach to get software inventory depth.

### 3. ServiceNow Horizontal Discovery (MID Server Probes/Patterns – Agentless)

**How it works:** ServiceNow’s horizontal discovery is an **agentless scanner** that uses a distributed component called the **MID Server** to probe devices in your network (on-prem or cloud) and identify what they are and what software they have. For software asset management, ServiceNow Discovery can be configured to specifically collect installed software from each machine it finds. This is typically done by running specific **probes or patterns** on the target system – for example, connecting to a Windows machine via WMI or PowerShell to list all programs, or to a Linux machine via SSH to list installed packages (rpm, dpkg, etc.). There is a platform property or pattern setting that controls whether software packages are collected; when enabled, *after identifying a CI, the discovery will execute an extra step to gather the list of installed software*. Those results are then stored as Software Installation records in ServiceNow and normalized for SAM. In essence, ServiceNow Discovery is doing what SCCM’s agent does, but remotely: logging into the machine (with provided credentials) and querying it for software data.

For example, on a Windows server, ServiceNow’s MID Server might use WMI queries like `SELECT * FROM Win32_Product` or read the Uninstall registry keys to get all installed applications. On Linux, it might run commands like `rpm -qa` or `dpkg -l`. This is done in real time during the discovery scan. The “probe” is the query sent, and the “sensor” is the logic that interprets the results and creates records in the CMDB. Modern versions use pattern-based discovery (which encapsulates these steps in patterns), but the concept of **agentless interrogation** remains the same.

**Capabilities:** The agentless approach via MID server can **discover virtually any IP-connected device** given the right credentials and access. For SAM specifically, it means you can gather installed software from:

* **Windows servers or PCs** (via WMI, WinRM, or SSH for Windows if configured, etc.),
* **Linux/Unix servers** (via SSH commands),
* **Mac OS devices** (if reachable via SSH and credentials, though in practice Mac discovery is rarer without an agent),
* **Network devices or appliances** (though these typically don’t have “software installs” beyond firmware, which is not in scope for SAM license management).

This method is extremely powerful for a heterogeneous environment or when you don’t have an existing inventory agent on systems. It can also uncover software on servers that might not be managed by SCCM or other tools. The data gathered (lists of installed software) is the same type of data we discussed: application name, version, etc., which SAM then normalizes. One key distinction: out-of-the-box **agentless discovery does not automatically capture usage metrics** (e.g. last used date of an application) – it’s a snapshot of what is installed at the time of scan. It can, however, be scheduled frequently (though not typically as frequently as an agent heartbeat) if needed for near-real-time data.

**Complexity and Setup:** Setting up ServiceNow Discovery requires more effort compared to just pulling from SCCM. The main component is deploying one or more **MID Server** agents in your environment. A MID Server is a lightweight Java service that runs on a Windows or Linux box in your network and brokers communication between ServiceNow and target devices. For an **on-premises network**, you would install a MID Server on a server that has network access (“line of sight”) to the devices you want to scan. For **Azure** or other clouds, it is recommended to also deploy a MID Server within those cloud networks (for example, a MID Server running on an Azure VM inside your virtual network) so it can reach the private IPs of other VMs. Best practice is to **place MID Servers in each network segment or cloud VNet** that you want to discover, to avoid discovery traffic having to traverse firewalls excessively.

Once MIDs are in place, you must configure **credentials** in ServiceNow for each type of system (Windows admin credentials, Linux SSH keys or accounts, etc.). You also define **IP address ranges or cloud subnets** for the MID to scan. Discovery can then be scheduled (e.g. nightly or weekly scans of certain subnets). When a scan runs, the MID will ping IPs, attempt to connect (WMI, SSH, etc.), identify the OS, then run the software discovery steps if enabled.

**Network and Firewall Considerations:** For agentless discovery to work, the MID Server must be able to reach target devices on the necessary ports/protocols. This often means opening firewall ports: e.g., **WMI uses TCP port 135 (RPC endpoint mapper) and dynamic high ports**, or **WinRM uses TCP 5985/5986** for PowerShell Remoting, and **SSH uses TCP 22**. In an on-prem network, if the MID and targets are on the same LAN or VPN, this may be open by default. Across subnet boundaries or to Azure, you’ll likely need to allow the MID’s IP to reach those ports. For Azure, if using IP-based discovery (rather than just Azure API), ensure either the MID is in the same VNet or appropriate **NSG (network security group) rules** allow the MID’s IP to access the VMs on the required ports. If you only do API-based cloud discovery, only port 443 outbound is needed, but here we’re talking about the deeper OS-level discovery. So yes, **“line of sight” is required between the MID and targets** – they must be able to communicate over the network. If there is a firewall between them, specific ports must be opened or a VPN/Direct Connect established.

Deploying discovery also involves tuning: deciding how frequently to run scans, how many MID threads to allow (to control load), etc. It’s not “set and forget” to the same extent as an agent, but once configured properly, it runs automated scans as per schedule.

**Pros:**

* *Comprehensive Coverage (Agentless):* You can discover **any device** with an IP address as long as you have credentials and network access. This is great for completeness – e.g., finding that one Linux box under a developer’s desk that isn’t in SCCM, or picking up software on a SQL Server in Azure that the SCCM agent wasn’t installed on. It’s all centralized under ServiceNow’s control.
* *No Agent Deployment:* The targets don’t need any special software installed (no agent on each target). This is beneficial in environments where you can’t or don’t want to install agents on servers (common for some DMZ or appliance-like systems). ServiceNow uses standard protocols (WMI, SSH) that many OS already support.
* *Rich Data and Relationships:* Besides just software, Discovery will also collect hardware details and can even map relationships (like what software is running on which server, and how servers connect). For SAM we focus on software, but it’s worth noting that agentless discovery can simultaneously gather info useful for other efforts (CMDB completeness, dependency mapping for service mapping, etc.). For example, discovery might find that a certain software is actually an Oracle DB instance and capture its Oracle listener details, etc., which SAM can then link to Oracle licensing needs. This **breadth of insight is greater** than what SCCM provides (SCCM is mostly focused on software installs and basic hardware, whereas ServiceNow discovery sees network connections, running services, etc. as well).
* *Immediate Data in ServiceNow:* The data goes straight into ServiceNow in real-time during the scan. There’s no intermediate database (like SCCM) that you have to sync; thus, once a scan finishes, you see the latest software installs in the CMDB. You can schedule scans at whatever frequency is feasible – some organizations do nightly for servers. If an urgent compliance check is needed, you could run an on-demand discovery on a subset and get results immediately.
* *Cross-OS and New Devices:* Agentless discovery is very useful for non-Windows OS (Linux, UNIX) and for *newly discovered devices*. If someone adds a server and forgets to put an SCCM agent on it, ServiceNow can still catch it via IP scan. It’s a safety net for unknown assets.

**Cons / Trade-offs:**

* *Higher Setup Complexity:* Compared to using existing SCCM data, deploying discovery has a learning curve. You need to plan MID Server placement (possibly deploying one in Azure, one on-prem, etc.), configure several credentials, and set up schedules. There’s also a need to adjust firewall settings unless the MID is already in the same open network as targets. This could mean coordinating with network/security teams – for example, opening RPC ports or SSH access from the MID to all servers – which can be time-consuming.
* *Network “Noise” and Impact:* Agentless discovery by nature generates network traffic. The MID will perform **port scans and probes** in the specified IP ranges. If misconfigured (scanning too large a range or too frequently), it can look like a port scan attack. Even properly configured, it will trigger a flurry of connections during the discovery window – e.g., establishing WMI sessions on many machines. In most networks this is fine, but it does add load to the network and target systems. Each target will have to respond to queries (which uses a bit of CPU and memory on that target). Typically this overhead is small – e.g., a WMI query for installed software might momentarily spike WMI process usage on a server and use some bandwidth to send the results back. **It’s usually not disruptive** (a ServiceNow expert notes that while the MID might hit 100% CPU, the target being scanned should not max out – it just uses some resources to respond to WMI queries). Still, some administrators are wary of anything that remotely “pokes” their servers. You might hear concerns about “scanning noise.” We’ll discuss in a later section how to minimize this, but it’s a consideration: discovery, if done in broad daylight or too often, could flood logs or network monitors.
* *Credentials Management:* Giving a service account the rights to read software installs on every server can be sensitive. For Windows, typically a domain account with local admin or WMI access is needed; for Linux, an account with `sudo` to run package queries. These need to be stored (encrypted) in ServiceNow and maintained. There’s security overhead to ensure those creds remain secure and up-to-date.
* *Coverage Gaps for Offline Devices:* Agentless scans are schedule-based; if a device is powered off or not reachable at scan time, it gets missed until next scan. An SCCM agent, by contrast, caches data and sends when connected. This is relevant for laptops or VMs that might be transient. For mostly server environments that are always on, this is less of an issue, but for e.g. developer VMs that start/stop, you might miss them if scanning at the wrong time (unless you integrate with cloud API or other triggers).
* *No built-in usage tracking:* Discovery will list what is installed, but not whether it’s been used recently (aside from perhaps install date). For SAM optimization (like reclaiming unused software), you would need to integrate usage data from elsewhere (SCCM usage logs or deploy ServiceNow’s agent to gather usage). Agentless alone doesn’t collect that telemetry.

**Use Case Fit:** In the given environment, agentless discovery via MID is likely already partially in use (“Discovery is functional for Azure tenants” likely means they have run cloud discovery, and possibly some infrastructure discovery). The user is mainly Azure-based, with some on-prem. A sensible approach is to **use ServiceNow Discovery to cover any gaps left by SCCM**. For instance, if the Azure environment contains Linux VMs (which SCCM 2016 wouldn’t cover well), a MID Server in Azure can SSH into those and pull software info. Or if there are Windows VMs in a workgroup (not in the domain/SCCM), MID can still discover them if given credentials. The on-prem small footprint could also be scanned by a MID Server to catch any device not in SCCM.

In a lot of real-world cases, companies use a **hybrid approach**: SCCM for Windows PC and server inventory, and ServiceNow Discovery for non-Windows devices or to double-check servers. This is feasible because ServiceNow can reconcile data from multiple sources into one CMDB entry (using the IRE rules to avoid duplicates). Indeed, **ServiceNow’s own guidance** suggests SCCM and agentless discovery complement each other – SCCM bringing in user-centric info from an agent, and Discovery covering everything else agentlessly.

### 4. Device-Initiated Agent-Based (ServiceNow Agent Client Collector)

**How it works:** Beyond using third-party agents like SCCM, ServiceNow has introduced its own **Agent Client Collector (ACC)** for an agent-based discovery and monitoring approach. As of the Yokohama timeframe, ACC is a relatively new but growing option that involves installing a **ServiceNow agent** on target machines. This agent runs as a service on the endpoint and collects data (including installed software and usage metrics) which it then pushes up to the ServiceNow instance (usually via the MID Server acting as a relay or via the instance API). Essentially, it inverts the discovery model: instead of the MID server polling the device, the agent on the device *initiates communication* out to ServiceNow. This significantly reduces the need for open inbound ports to the device; as long as the device can reach the ServiceNow instance (or MID) on HTTPS, it can report in.

The ACC for Visibility (often abbreviated **ACC-V**) is the component focused on collecting inventory and configuration from endpoints. For SAM, ACC-V can collect **installed software lists and software usage data** from each computer where it runs. For example, once the agent is installed on a Windows laptop, it can immediately gather the list of programs installed and send that to ServiceNow (creating the same Software Installation records as discovery would). It can also track when each application is run, updating a “last used” timestamp for each software, enabling true usage metering. This is very useful for reclamation – you can know not just that Visio is installed on a machine, but that it hasn’t been opened in 120 days, indicating a candidate for removal.

**Capabilities:** ACC combines the thoroughness of an on-device agent with direct integration to ServiceNow. Key capabilities for SAM include:

* **Complete software inventory per device:** The agent will pull all installed software entries from the OS (similar to SCCM’s agent or discovery’s probe) and report them. This covers whatever the OS sees (Add/Remove Programs on Windows, etc.). According to ServiceNow documentation, when configured correctly, each endpoint will register itself in the CMDB and list its installed software in the SAM tables. In effect, it populates the same data as other methods, but continuously.
* **Software usage tracking:** ACC can track application usage natively. For instance, it can note when an .exe is launched and record usage stats. These are fed into the SAM **Software Usage** tables. This addresses the usage aspect that agentless methods miss.
* **Cross-platform:** The ServiceNow agent can be installed on Windows, Linux, and possibly Mac (as of late 2024, Windows and Linux were supported for ACC, and Mac support was in progress). This means you could have one unified agent for all, instead of separate tools for different OS.
* **Near real-time updates:** Because the agent runs continuously and can send data at frequent intervals or on change, ServiceNow gets updates more immediately. If you install a new software on a server, the agent might report it within minutes, as opposed to waiting for the next scheduled discovery or SCCM cycle. This can make your SAM data very current.

**Complexity and Setup:** Using ACC requires deploying the agent to each target system, which is a significant effort (similar to any endpoint management agent deployment). You need to plan installation (via SCCM itself, or Intune, or an RMM tool, etc.). Additionally, you must activate the ACC plugins in ServiceNow (Agent Client Collector plugin, and policies for what data to collect). A MID Server is still needed in the environment as a coordination point for agent connectivity (the MID doesn’t scan the device, but the agent might communicate via the MID). Firewall-wise, the agent typically makes outbound calls (HTTPS) to the MID or instance, so you ensure devices can reach your ServiceNow instance URL or MID on the required port (usually 443). There’s no need to open inbound ports on each device for discovery purposes, which is a plus in restrictive networks.

The complexity here lies in organizational change: deploying a new agent might require approvals and security reviews. The agent will consume some resources on each endpoint (generally light, but it’s additive to any existing agents like SCCM, antivirus, etc.). Administrators might ask what this agent does, how to update it, how it’s secured. ServiceNow’s ACC is evolving, so organizations need to be on a fairly recent release (which Yokohama is) and possibly have the ITOM Visibility license (ACC was initially part of ITOM, but its data can be used for SAM).

**Pros:**

* *Minimal Network Noise:* Since the device itself reports in, there are **no broad network scans**. This eliminates the “scanner” footprint – no ping sweeps or port probes. From a network perspective, each device just opens an outgoing HTTPS connection to ServiceNow (which is typically allowed even from remote locations). This is very firewall-friendly. As long as an endpoint can reach the internet or your SN instance, it can report without special firewall holes per se. This is particularly great for **remote offices or machines off-network** (e.g., a laptop at home with no VPN – it can still send data to SN over the internet).
* *Low Endpoint Disruption:* An agent can gather info locally with optimized calls. It might use OS APIs directly rather than remote WMI which can be heavier. The impact on the endpoint is typically negligible – and since it’s not a remote scan, it won’t show up as a login from an external system. Everything is local system activity. No repeated authentication handshake for WMI each time – the agent is persistently running. This reduces the chance of triggering security alerts on the device.
* *Timeliness and Frequency:* You can get software changes in near real-time. If someone installs or uninstalls software, the agent can send an update event soon after. You’re not limited to periodic polling. This is ideal for environments with rapid changes or where license compliance is tightly monitored.
* *Usage Data:* This is a big plus – knowing actual usage elevates SAM from a static compliance check to an optimization tool. Only with an agent (or something like SCCM metering) can you get this data. ACC provides it out-of-the-box for software it monitors. For example, you can see that out of 100 Visio installs, 30 haven’t been used in 90 days – enabling automatic reclamation via SAM.
* *Unified Solution:* Using ServiceNow’s own agent means one integrated platform for discovery and asset management. It avoids reliance on third-party inventory sources. Over time, this could simplify the architecture (e.g., maybe not needing SCCM inventory if ACC covers it, although SCCM might still be used for software deployment, etc.).

**Cons / Trade-offs:**

* *Agent Deployment Overhead:* Installing the ACC on every relevant system is a project. If the environment is large, one would typically use an automated software distribution method (could ironically use SCCM or Intune to push the ServiceNow agent). This is doable but requires coordination. Every endpoint now has one more agent to oversee. Some organizations resist “yet another agent” due to potential conflicts or resource usage.
* *Immaturity and Coverage:* As of Yokohama (late 2024), ACC was relatively new. Early adopters reported that usage was not yet widespread. It’s rapidly improving, but there might be edge cases or features not yet on par with older tools. For instance, Service Mapping capabilities were not yet in ACC as of early 2025, and if your SAM process also relies on certain discovery details (like complex Oracle DB discovery for license needs), you’d want to ensure the agent can collect those. The community consensus has been that **for servers, ServiceNow’s horizontal discovery could do “much more” than ACC in terms of depth**, and if you already have Discovery working, ACC on servers might not add much SAM value beyond what you have. For end-user computers, however, ACC was seen as very useful especially for usage data.
* *Licensing/Cost:* Depending on your ServiceNow licensing, using ACC might require ITOM licenses or additional purchases (since it’s part of the ITOM Visibility/Health product family, though its data benefits SAM which is ITAM). This is something to confirm with licensing – the SAM Professional SKU alone might not entitle full use of ACC agents on all servers without ITOM. This could introduce cost considerations.
* *Redundancy with SCCM:* If you already have SCCM, deploying ACC might seem redundant. Both would be doing inventory on Windows devices. In the long run, an organization might choose one or the other. But in the interim, you might have two agents doing similar jobs, which isn’t efficient unless you’re transitioning. Some organizations may prefer to stick with the known (SCCM) until ACC proves itself fully. On the other hand, ACC would bring non-Windows and usage into scope which SCCM might not.

**Use Case Fit:** For this specific user scenario, ACC could be an **optional enhancement**. Given they have SCCM feeding data, one might not rush to deploy ACC on all servers right away (especially since agentless discovery and SCCM cover inventory). However, ACC could be very beneficial for **end-user workstations** if SAM Pro is extended to them and usage reclamation is a goal. In an Azure-centric environment, if there are cloud VMs that are not managed by SCCM or easily scanned (say, developer machines or geographically dispersed assets), putting the ServiceNow agent on them would instantly bring them into SAM scope without fiddling with firewall rules. It’s a modern approach that is likely to become more common. The ideal strategy might be: use ACC on laptops and any unmanaged servers, while continuing agentless discovery for heavy-duty server discovery and SCCM for those established Windows servers – essentially a mix. In fact, one expert suggests a mix as the “best at the moment”: **ACC + SCCM for workstations (to get both installs and usage), and ServiceNow Discovery (plus maybe ACC) for servers**.

The combination of methods can be tailored: e.g., do we need ACC on servers that are already covered by Discovery? Probably not for inventory alone, unless usage tracking on servers is crucial (server software usage is less of a concern usually than user software). Do we need SCCM if we deploy ACC fully? Perhaps not, but SCCM does other things (software distribution, patching) so it might stay for operational reasons.

To summarize this section: **ServiceNow SAM can ingest software data from SCCM, Azure, its own MID-driven discovery, or its own agent.** Each has strengths. Below is a matrix comparing these approaches by complexity and capability, followed by a discussion on network impact (“noise”) and recommendations.

## Comparison Matrix: Discovery Method vs. Complexity and Capability

To visualize the trade-offs, the following table maps the **implementation complexity** against the **software discovery capabilities** of each approach. This can help in deciding which method(s) to use in a given environment:

| **Discovery Method**                             | **Implementation Complexity** (Deployment & Connectivity)                                                                                                                                                                                                                                                                                                                                                                                                             | **Software Inventory Capability** (Coverage & Detail)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **SCCM Integration (Service Graph Connector)**   | **Low** – Leverages existing SCCM agents on devices (no new agent rollout). Must install/configure the SCCM connector in ServiceNow and schedule data imports. Requires access to SCCM database or API. Minimal firewall changes (outbound from MID to SCCM). Relies on maintaining SCCM infrastructure.                                                                                                                                                              | **High for Windows clients** – Provides detailed installed software per device via SCCM’s inventory. Covers all software that SCCM captures (not limited to specific titles). Also brings in user information (which user uses the device) and can include software usage if metering enabled. Data is automatically normalized for SAM reconciliation. *Non-Windows devices* are out of scope unless SCCM managed them.                                                                                                                                                                                  |
| **Azure Native Discovery (Cloud API)**           | **Low** – Configuration involves setting up an Azure service principal and a MID Server with internet access. No agents on target VMs, no per-device credentials needed for basic inventory. Only requires outbound HTTPS (443) from MID to Azure APIs. Very little on-prem infrastructure needed aside from the MID.                                                                                                                                                 | **Low (for software visibility)** – Discovers cloud VM resources and metadata (OS type, hostname, IP, etc.) but **does not retrieve installed software by default**. Ensures all Azure VMs are tracked in CMDB, preventing blind spots, but you must combine with another method for actual software details. Essentially, it’s great for inventorying machines, not the software on them.                                                                                                                                                                                                                |
| **ServiceNow Discovery (MID Server, Agentless)** | **Medium-High** – Requires deploying one or more MID Servers in each network segment (and/or Azure VNet) that has targets. Need to configure credentials (Windows, Linux, etc.) and open relevant firewall ports (e.g., WMI/DCOM or WinRM for Windows, SSH for Linux) between the MID and targets. Also involves setting up discovery schedules and maintaining MID Server health. More upfront effort, but once in place can be reused for multiple discovery tasks. | **High (Broad Coverage)** – Discovers installed software on any reachable device, regardless of OS. Creates Software Installation records for all software found (similar to SCCM’s list) which are normalized for SAM. Suitable for Windows, Linux, etc. Depth of detail is good (names, versions, publisher). However, **no usage data by default** (it’s snapshot-based). Also discovers additional context like hardware details and relationships (beneficial for broader ITOM, though not directly used in license counts). Best for servers and any device not already covered by an agent source. |
| **ServiceNow ACC Agent (Device-Initiated)**      | **Medium** – Requires deploying ServiceNow’s ACC agent on target systems (endpoint installation needed). Must manage distribution via software deployment tools. The agent communicates out to ServiceNow (typically via MID), so ensure endpoints have outbound access (443) to the instance or MID. No need to configure numerous credentials or open inbound ports to each device. Need to manage agent policy and updates.                                        | **High (Deep Insight + Usage)** – Collects complete installed software inventory on each device (comparable to SCCM/Discovery data) and additionally captures **software usage metrics** (last used timestamps, frequency). Near real-time updates as devices report changes. Cross-platform coverage (Windows, Linux, etc.) with a single agent. Excellent for tracking user workstation software and utilization. For server software, provides inventory (and some runtime info), though agentless discovery still covers broader discovery of dependencies.                                           |

**Table: Capability vs. Complexity of ServiceNow SAM Discovery Methods**

As shown above, **SCCM and ACC (agent-based)** methods excel in depth for known systems (especially giving usage or user context), with relatively low network complexity (since they’re device-initiated), whereas **ServiceNow’s MID-based discovery** has higher setup overhead but can reach places and systems the others might miss (and doesn’t require an existing agent). **Azure API integration** is low-hanging fruit for basic coverage, but needs to be coupled with one of the other methods to serve SAM’s purposes.

## Network “Noise” and Endpoint Impact: Probes vs. Agent Communication

A common concern in any discovery approach is: *What is the impact on my network and devices?* We will clarify the level of “disruption” or “noise” caused by ServiceNow’s discovery probes compared to agent-based (device-initiated) methods.

**Agentless Probes & Sensors (MID Server):** When ServiceNow Discovery scans a subnet, the MID Server performs several network actions: ping sweeps, port scans (to identify open ports/services), and then specific probes like WMI queries or SSH commands. This can produce noticeable network traffic, especially in large IP ranges. For example, a MID might send ICMP pings to hundreds of IPs, attempt connections on TCP 135/445, etc., which could appear in intrusion detection systems. Each target that responds will then get a deeper query (e.g., the MID establishing a WMI session to retrieve installed software).

From the **network perspective**, this is “noisy” in the sense that it’s a centralized scanner hitting many endpoints. However, ServiceNow Discovery is generally polite – it’s not an aggressive port scanner beyond what’s needed. You can configure it to limit the number of simultaneous probes and use schedules to avoid peak hours. The bandwidth used is usually minimal relative to modern networks (the data returned e.g. a list of software might be a few KBs or tens of KBs per machine). It’s more the number of connections that can raise eyebrows. If you have firewall logs on each server, you’ll see the MID server’s IP contacting it during discovery windows. Some highly sensitive systems might flag this as unusual if not informed.

On the **endpoint (target)** side, agentless queries do consume some resources. For instance, when the MID asks Windows for installed software via WMI, the WMI service on that Windows machine will use CPU cycles to enumerate the Uninstall registry keys. If the machine is extremely busy or the WMI query is heavy, there could be a slight performance hit. Typically, this is minor – one community expert mentioned that 100% CPU on the MID is expected (it’s doing a lot of work), but on the target being scanned, it’s not expected to spike to 100%, just some resources to respond. In practice, most well-maintained servers handle discovery probes without issues; the load is comparable to an administrator remotely running a system info query. That said, if you scan, say, 2000 PCs at once, each PC might see a brief blip in CPU/RAM for WMI, which if all happen concurrently could show as a broad event. Staggering scans mitigates this.

The **“disruption”** from MID-based discovery is mostly about timing and scope: if run during production peak hours, a database server might have a slight extra load answering queries, or an ill-timed scan could clog a slower network link if not planned. These risks are manageable by scheduling (e.g., do nightly discoveries for servers, or smaller targeted scans for subnets). Also, discovery can be configured to be **target-specific** – you don’t have to continuously scan everything if it’s already known. For instance, if Azure API already brought in a list of VMs, you could target discovery only to those nodes, avoiding blind sweeps of unused IP space (reducing noise).

**Device-Initiated (Agent-Based) Communication:** In agent-based models (SCCM or ACC), the communication pattern is reversed. The endpoint agent wakes up on a schedule (or trigger) and sends data to a central server. This tends to be **much quieter on the network** because each agent talks individually to the server with a small payload. There’s no broad scanning, no port probing – just a client-server communication typically over a single port (usually HTTPS or an RFC-compliant port like 10123 for older SCCM, etc.). For example, the SCCM agent might send inventory delta once a day; that’s one connection per device to the SCCM server. Similarly, the ServiceNow ACC agent might maintain a lightweight heartbeat and send updates. These are background flows easily handled by existing network bandwidth.

On the endpoint itself, the agent runs its inventory locally. This can be optimized by the OS – reading the list of installed programs from the registry is very fast and only diffs are sent. Agents are generally designed to be low-impact (running with low priority, etc.). They also often run at scheduled times (SCCM’s “hardware inventory scan” might run at 1am local time by default). So the user or server workload typically isn’t disturbed. The “noise” internally is just another process doing a quick check.

One important point: **Credential vs. Trust:** Agentless requires providing admin credentials to the MID Server to access the target, which if those credentials are misused or intercepted could be an issue – but ServiceNow stores them securely and the MID uses them only for discovery. Agent-based avoids sharing admin creds across the network; the trust is in the agent software itself, which is generally more secure in terms of not exposing a domain-wide credential for each query.

**Security monitoring differences:** With MID discovery, you might see logs like “Successful login from MIDserver1 as Administrator at 2AM” on each server – which could alarm some security folks until they realize it’s the discovery account. With an agent, the activity is local so it doesn’t generate those remote login events on each host. Instead, you’d see the agent service starting and running, which is expected. So agent-based is often considered *stealthier* from an audit perspective (no repeated remote logins), whereas agentless is considered *transparent* (no need to install anything, but it does leave a trail of remote accesses).

**Impact on network performance:** According to experienced users, ServiceNow discovery traffic rarely causes *significant* network degradation. It’s more about ensuring you don’t schedule huge scans on slow links midday. The question posted in the community about negative network impact had the answer of referencing resource utilization guidelines – meaning it’s known and documented how to size/tune discovery so it doesn’t overwhelm networks. As a rule of thumb: if your MID Server is in the same LAN as targets, the traffic stays local and is negligible; if scanning over a WAN (not recommended), then you schedule carefully. With agent-based, the traffic is so distributed and trickling that it’s almost never a concern unless you have tens of thousands of agents all reporting at once (even then, it’s often a few MB per agent at most for full inventory).

In summary, **agentless probes can introduce some network noise and small endpoint workload at scan time**, whereas **agent-based communications are quieter and more continuous**. The latter shifts the work to the endpoint in a controlled way and avoids the big “scan event” pattern. Neither approach should “hammer” a device in a destructive way if properly configured – ServiceNow’s methods are used in large enterprises successfully. But if one’s goal is to **minimize any scanning footprint**, leaning on agent-based (SCCM or ACC) is the better choice, using agentless only where needed.

## Recommendations: Balancing Complexity, Coverage, and Noise

Considering all of the above for the given environment (primarily Azure, SCCM in place, small on-prem, SAM goals), here are recommendations to **maximize software asset visibility while minimizing network disruption**:

* **Leverage SCCM where strong:** Continue using the SCCM integration as the primary source for Windows servers and client machines that are managed by SCCM. This provides a reliable feed of software installs with minimal additional effort or network impact (since it’s agent-driven and already frequent at 8x daily). It ensures user devices and domain-joined Azure VMs with the SCCM agent stay updated in SAM with almost no “noise” from ServiceNow’s perspective. The SCCM data will feed SAM’s normalization and compliance engine effectively. Just maintain the SCCM connector and monitor that it’s pulling data correctly. This covers the bulk of traditional software discovery needs for Windows.

* **Use Azure Cloud Discovery for visibility, not for software detail:** Keep the Azure integration active to catch any new VMs or cloud resources. This zero-footprint discovery (API calls only) is worth having for completeness. It will not give you software lists, but it will ensure your CMDB/SAM knows about every compute instance in Azure. Think of it as “inventory of targets.” All newly created VMs in Azure will automatically appear in ServiceNow (typically within a day or sooner, depending on schedule). This way, even if an admin stood up a VM and didn’t enroll it in SCCM or tell anyone, you’d know it exists. Then you can decide how to handle its software. The Azure integration is low complexity and low noise – no reason not to use it. It’s already in place and should remain as a foundational layer.

* **Deploy a ServiceNow MID Server in Azure for deep discovery (targeted):** To address any Azure VMs that are **not** covered by SCCM (e.g., perhaps Linux servers, or Windows servers that for some reason don’t run the SCCM agent), deploy a MID Server within the Azure environment and configure **targeted horizontal discovery** for those systems. This MID can be placed on a small VM in the same virtual network or peered network as your Azure servers. By being in Azure, it avoids traffic going over the internet or VPN for discovery – it can reach local IPs directly. Use this MID to run discoveries on specific IP ranges or specific Azure VMs. Since the Azure plugin already lists the VMs, you can use that list to guide discovery – for example, schedule a nightly pattern-based discovery on all Azure VMs of type Linux, or on any Windows VMs that show as not having SCCM agent. Credentials: you’d provide an SSH key for Linux, and possibly a Windows admin credential if needed (for any Windows VM you want to scan agentlessly). The key is to **scope the discovery** – you don’t necessarily need to scan the entire Azure address space if the cloud integration can trigger or inform which nodes to scan. This targeted approach will minimize unnecessary probes (hence reducing noise). Also consider using **WinRM (PowerShell)** for Windows discovery rather than WMI/DCOM, as WinRM can work over a single port (5985/5986) which is easier to secure through firewalls (and can be locked down to the MID’s IP). This reduces the “ephemeral port” issue of WMI. ServiceNow Discovery supports using WinRM and PowerShell patterns for Windows, which can be configured in credentials.

* **Mid Server for On-Prem:** For the small on-premises footprint, if SCCM is already covering those machines, you might not need heavy discovery. However, if there are a few stray on-prem servers or devices not in SCCM (perhaps network devices or a Linux NAS), a local MID Server can discover them. Given the footprint is small and dispersed, you could either place a single MID in the main datacenter and open firewall routes to branch locations (if those few servers are reachable), or even temporarily deploy a MID in those locations if needed. Since on-prem is small, the complexity here can be kept minimal – perhaps run discovery on-demand for on-prem devices if SCCM can’t cover them. But in many cases, SCCM probably covers on-prem Windows servers too, so the MID’s role on-prem might just be supplementary.

* **Introduce the ServiceNow ACC agent selectively (if beneficial):** To maximize visibility especially into software **usage**, consider deploying ACC agents on *end-user computing devices* and possibly on certain server groups where usage tracking could drive optimization. For example, if the organization has many developer VMs or test servers that people install software on, an ACC agent on those could feed usage data to SAM, enabling reclamation of rarely used licenses. Similarly, for knowledge worker laptops that have expensive software (Adobe Creative Suite, etc.), ACC can report if those apps are actually being used. Since the question is more about software discovery, ACC is not strictly required if SCCM and discovery suffice for inventory; but **for completeness in a SAM practice, usage data is the next step**. The noise impact of ACC is low, so you can deploy it without worrying about scans. The complexity is the rollout – perhaps pilot it on a subset of machines first. Over time, if ACC proves its worth, you might expand it and even potentially reduce dependency on SCCM for inventory (though SCCM might remain for other reasons).

* **Use a Hybrid Data Reconciliation Strategy:** In ServiceNow SAM, you can ingest data from multiple sources. Ensure you define in the **Data Source precedence** that, say, SCCM is the preferred source for Windows software installs, and perhaps Discovery is used for others. ServiceNow’s Identification Reconciliation Engine (IRE) will de-duplicate so that you don’t double-count the same installation from two sources. For example, if a server is scanned by both SCCM and Discovery, you might see two records for the same software – IRE can merge or you can configure SAM to use one source. One approach is to designate SCCM as authoritative for Windows CIs that have SCCM (with a field indicating last inventory source), and designate Discovery for those that don’t. Clean up any old data that is no longer updating (the community post advised to remove outdated discovery data if SCCM is now managing that CI, to avoid stale records). This will ensure your license reconciliation is counting each installation only once.

* **Minimize Discovery “noise”:** To specifically address the noise/disruption concern – implement discovery in a **controlled manner**:

  * Schedule discovery scans for **off-peak hours** where possible (e.g., midnight scans for servers). This way, even if there is a burst of network activity, it’s unlikely to impact users or critical daytime processes.
  * Use **smaller discovery batches** rather than one huge schedule for all IPs. For instance, divide Azure VMs into a few groups and scan them in staggered windows, or use dynamic discovery (ServiceNow Discovery can be triggered by the Cloud API findings to do one VM at a time after it’s created, instead of scanning all). This avoids big spikes.
  * Enable **sniping**: focus on known IPs from the CMDB rather than blind scanning IP ranges. If Cloud Discovery already populated CI IP addresses, the MID can target those directly. This avoids sending packets to unused IPs (reducing network chatter and scan duration).
  * Configure **credentials correctly** to prevent failed login noise. For example, if a credential is wrong or not applicable, the MID might try multiple times, which is noisy and can lock accounts. Ensure each IP range has the appropriate credential assigned (Windows creds for Windows IP ranges, etc.) to minimize authentication failures.
  * Monitor the **Discovery logs and MID performance** initially. If you see that certain probes take too long or cause errors, tune them. For instance, if software discovery on a particular huge server is slow, you might adjust or increase MID threads to get it done faster and not tie up that server.
  * Educate the network/security team beforehand: let them know the MID servers’ IPs and what ports they will use, so these scans are expected and not mistaken for malicious activity. Many teams will be okay with it once they know it’s an authorized tool doing inventory.

* **Maintain a Capability-Complexity Balance:** Not every environment needs every method fully deployed. In this case, a reasonable balance is:

  * **SCCM for the bulk** of Windows inventory (low incremental effort, high reward).
  * **Cloud API for infrastructure awareness** (no impact, covers cloud sprawl).
  * **Targeted MID discovery for gaps** (some effort, but focused on areas SCCM can’t reach, e.g., Linux or un-managed instances).
  * **ACC for strategic usage tracking** (medium effort, high value for optimizing licenses, but can be phased in gradually).

  This combination minimizes redundant work. Each method fills a specific need:

  * SCCM and ACC *avoid network scans* by using agents (keeping noise down).
  * Discovery via MID is used surgically, not indiscriminately, to cover non-agent endpoints (ensuring you still capture everything in the SAM scope).
  * Azure integration quietly feeds the system of record.

* **Regularly review results and adjust:** As SAM operates, review the Software Installation records and see if there are systems not reporting data. For example, you might find some Azure VM that shows up via cloud discovery but has no software listed – that’s a flag to either deploy an agent there or include it in a discovery scan. By plugging such holes, you improve coverage. If you find the overhead of one method is too high for the benefit, consider alternatives (e.g., if a Linux box is hard to reach via MID due to network constraints, maybe install the ACC agent on it or use Azure Arc to get inventory and integrate that).

In conclusion, **ServiceNow SAM in Yokohama can comprehensively inventory and reconcile software licenses when fed with quality discovery data**. The best practice in a modern hybrid environment is often a *hybrid discovery approach*: use existing **agent-based tools like SCCM** for what they’re best at, employ **ServiceNow’s agentless discovery** for the uncaught areas and infrastructure mapping, and consider **agent-based enhancements (ACC)** for usage insights. By doing so, you get the richest dataset for SAM with the least disruption.

The capability-to-complexity matrix and analysis above indicate that each method has its role – it’s not necessarily about choosing one, but rather orchestrating them. The end goal is a near real-time, accurate picture of all software in your environment (from on-premises to Azure cloud) and the ability to reconcile that against your licenses. With Yokohama’s SAM features and these discovery methods in tandem, you can achieve that goal while keeping network noise and administrative overhead to a practical minimum.

**Sources:** The recommendations and comparisons are grounded in ServiceNow’s documentation and community expertise, including guidance on SCCM integration providing detailed per-device software data, notes on SCCM vs. Discovery usage in practice, requirements for MID Server placement and firewall access, ServiceNow Discovery’s ability to retrieve installed software lists via probes, and the capabilities of the ServiceNow ACC agent for collecting software and usage information. These informed insights ensure a real-world view of SAM discovery in the Yokohama release.



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
