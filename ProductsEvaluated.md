# Products Evaluated:

| Product | Company | FedRAMP | GCC | Data | Collaboration | Agentic | Inline / Iterative UI |
|---------|---------|---------|-----|------|---------------|---------|----------------------|
| **Criteria** | N/A | FedRAMP High authorized or actively pursuing; must run in FedRAMP environment | Deployable in AWS GovCloud, Azure Gov, or equivalent isolated gov cloud | Semantic search across internal data + ability to query external APIs/web; native embedding generation preferred | Real-time collaboration features; ability for multiple users to work with AI simultaneously | Autonomous multi-step task execution; API/SaaS integrations; OS-level actions a plus | Integrated UI for iterative interactions; preferably within existing tools (e.g., Office apps, chat platforms) |Can execute tasks autonomously | Integrated UI for iterative interactions |
| Microsoft 365 Copilot | Microsoft | Azure OpenAI FedRAMP High authorized | Targeting GCC High availability by Summer 2025 | Internal M365 data, Bing web search, extensible to third-party via plugins | Real-time co-editing in Office apps (Word, Excel, Teams) | Can draft emails, schedule meetings, create plans, some workflow automation | Integrated directly into Office apps for inline suggestions and edits |
| Google Workspace Duet AI | Google | Workspace is FedRAMP High, Duet AI (Gemini) submitted for FedRAMP High | Available in Google Workspace Government Cloud | Internal Google Workspace data, limited direct web integration | Real-time assistance in Google Docs, Sheets, Gmail | Basic task automation (e.g., meeting scheduling, email drafting) | Inline suggestions and auto-completion in Google Docs, Sheets, and Gmail |
| Primer Delta & Command | Primer AI | Deployable up to FedRAMP High/IL5 via Palantir FedStart | ATO on JWICS for classified environments | Internal databases, document corpora, OSINT, news, social media | Team-based refinement of AI-generated reports and insights | Primarily research and synthesis, not direct task execution | Dedicated interface for querying and refining AI-generated content |
| Palantir AIP | Palantir | FedRAMP High authorized (Palantir Federal Cloud) | DoD IL5/6 for higher classification levels | Unified access to enterprise data, external feeds, strong ontology | Multi-user AI querying and supervision in Palantir workspace | Can update records, call APIs, run code, automate decisions with approval | Integrated into Palantir's data analysis interface for iterative querying and refinement |
| Moveworks AI Platform | Moveworks | FedRAMP Ready, pursuing FedRAMP Moderate ATO | Deployable in AWS GovCloud | Internal systems (IT, HR, knowledge bases), limited web integration | AI assistant in chat platforms (Teams, Slack) for real-time support | Workflow automation for IT/HR tasks, can resolve requests autonomously | Conversational UI in chat platforms allows for iterative refinement of requests |
| Amazon Bedrock & Kendra | AWS | Both FedRAMP High authorized in GovCloud | Available in AWS GovCloud | Can index internal documents, potential for web integration via custom development | Requires custom integration for collaborative features | Potential for custom agent development using AWS Lambda | Requires custom development for inline capabilities; primarily API-based |

And here's the updated Harvey ball rating table, including the new column:

| Product | Company | FedRAMP | GCC | Data | Collaboration | Agentic | Inline / Iterative UI |
|---------|---------|---------|-----|------|---------------|---------|----------------------|
| Microsoft 365 Copilot | Microsoft | ● | ◔ | ● | ● | ◑ | ● |
| Google Workspace Duet AI | Google | ◑ | ● | ◑ | ◑ | ◔ | ● |
| Primer Delta & Command | Primer AI | ● | ● | ● | ◑ | ◔ | ◑ |
| Palantir AIP | Palantir | ● | ● | ● | ◑ | ● | ◑ |
| Moveworks AI Platform | Moveworks | ◔ | ◑ | ◔ | ◑ | ◑ | ◑ |
| Amazon Bedrock & Kendra | AWS | ● | ● | ◑ | ○ | ◔ | ○ |

Legend:
○ = 1 (Minimal or No capability)
◔ = 2 (Basic capability)
◑ = 3 (Moderate capability)
● = 4 (Full or Advanced capability)

Note: The ratings for the new "Inline / Iterative UI" column are based on the information provided in the research and my interpretation of each product's capabilities. Microsoft and Google receive full marks due to their deep integration into widely-used productivity suites. Primer, Palantir, and Moveworks have dedicated interfaces that allow for iterative refinement, hence the moderate rating. AWS Bedrock & Kendra, being primarily API-based services, require custom development for inline capabilities, thus receiving the lowest rating in this category.
