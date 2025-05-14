# Key points
- SCCM can provide signfiicant SAM coverage with minimal disruption
- Cloud Discovery may provide additional coverage with minimal disruption
- Discovery beyond SCCM and Cloud Discovery will require a significant investiment in time and complexity with MID servers requiring line of sight across all multiple firewall layers, large coordination with infrasructure and security teams require and independent impact statements are required from each tower

# Initial Prompt
We need a deep understanding of Software Asset Management within ServiceNow, specifically target capability as of Yokohama release, focus on discovery.  We need to understand the capabilities in real world grounded terms and understand the potentially disruptive nature of any "scanning" or "probes" we may need, strongly consider MID requirements, line-of-sight MID Firewall Requirements.

We have in place SCCM 2016 reporting 8x daily
We have in place Azure Subscriptions with ServiceNow Cloud Management plugin and we get daily discovery from theoretically every subscription / tenant within our Azure environment and we are mostly an Azure shop

Consider the full capabilities of ServiceNow Software Asset Management and build a complexity to capability matrix

## Clarifying questions
Are you focused solely on software discovery (e.g., installed software and license reconciliation), or are you also concerned with hardware inventory, cloud cost visibility, and SaaS app management?
A: Software

Are you already using ServiceNow Discovery (or planning to), or are you relying only on SCCM and Azure integrations?
A: We currently have cloud discovery enabled for our Azure tenants, and we are mostly focused on Azure and a small but dispersed on-prem footprint, we currently have SCCM reporting from computers (Windows servers and laptops) We do not have any other experience with discovery, we have a robust firewall request process which we can follow as needed but we hope to ultimately balance complexity to benefits

Is there a need to analyze the impact of MID Server deployments across specific security zones or geographies?
A: No, but we must have an understanding of MID requirements, do we require line of sight (firewall) betweemn MID servers and on prem hardware or in Azure compute etc.

Should we include recommendations or configurations for minimizing scanning disruptions or firewall rule changes?
A: This is a key consideration, we must answer the question at what cost of noise comes adequate software asset manaegment scanning, how much is scanning and how much is devices indepdendently phoning home?

Understood. I’ll prepare a deep dive into ServiceNow Software Asset Management capabilities as of the Yokohama release, with a focus on software discovery. This will include an evaluation of discovery methods (SCCM, Azure, and ServiceNow-native options), MID Server requirements including firewall and line-of-sight considerations, and a complexity-to-capability matrix. I’ll also explore the trade-offs between agentless scanning, MID-based probes, and device-initiated reporting to highlight potential disruption vs. value.

# Research

### High Level Grok
I need a high level understanding of ServiceNow Software Asset Management, what technologies are used to scan or collect information on license usage within a typical environment?  What is the expected scope (computers / servers / services) and how disruptive should we expect any scanning to be?

