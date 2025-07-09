Story Number: STRY0022584
Short Description: ServiceNow Enabler: Yokohama Now Assist
Story Description: As an admin, I need to ensure updates and new functionality associated with the Yokohama release doesn't interfere or break existing functionality in our instance so that the upgrade results in benefits instead of issues.
Acceptance Criteria:
1. Now Assist

| Now Assist | [Link](https://docs.servicenow.com/bundle/yokohama-washingtondc-df3/page/release-notes/rn-combined/yokohama-washingtondc/yokohama-washingtondc-nowassist-release-notes.html) |
|------------|------|

Deeply research ServiceNow Now Assist Yokohama release notes and compile a coherent series of declarative statements describing the Now Assist offering/product/capabilities, these declarative statements will serve as statements of understanding for future AI consumption and response

ChatGPT said:
To make sure I provide the most relevant and accurate declarative statements about Now Assist in the Yokohama release, could you please clarify:

Do you want statements focused only on new or changed features in the Yokohama release, or should I include a full overview of Now Assist capabilities including pre-Yokohama features?

Is there a specific format, style, or tone you prefer for the declarative statements (e.g., bullet points, one-liners, grouped by capability)?

Should I focus on certain modules of Now Assist (e.g., Virtual Agent, Search, Agent Assist), or include everything mentioned in the release notes?








You said:
Do you want statements focused only on new or changed features in the Yokohama release, or should I include a full overview of Now Assist capabilities including pre-Yokohama features?
A: Full overview, as if this is our first time utilizing Now Assist, be thorough.

Is there a specific format, style, or tone you prefer for the declarative statements (e.g., bullet points, one-liners, grouped by capability)?
A: Straightforward, exercise your mastery of the economy of language to provide optimal understanding in the simplest, most straightforward manner.

Should I focus on certain modules of Now Assist (e.g., Virtual Agent, Search, Agent Assist), or include everything mentioned in the release notes?
A: We must exhaustively research everything mentioned in the release notes as the declarative statements you provide will serve as the foundation for continued development and operational work related to Now Assist

Be sure to include a special section devoted to the entitlement, installation and configuration of any and all plugins, you must comprehensively cover this topic as this will be used as a definitive guide to ramp from zero to full use of Now Assist.

ChatGPT said:
Understood. I’ll conduct an in-depth review of the Now Assist capabilities introduced and enhanced in the ServiceNow Yokohama release, along with foundational features and setup requirements.

I will provide a comprehensive set of concise declarative statements covering all functional areas, including Virtual Agent, Agent Assist, Search, Case Summarization, and more. A dedicated section will detail plugin entitlements, installation, and configuration necessary for initial setup and operational readiness.

I’ll let you know once the full set of statements is ready for your review.

Research completed in 14m · 31 sources · 35 searches
Comprehensive Overview of Now Assist in the ServiceNow Yokohama Release

Now Assist in ServiceNow’s Yokohama Release – Overview and Key Capabilities
Now Assist Overview
Now Assist is ServiceNow’s family of generative AI features integrated across the Now Platform. It uses large language models (LLMs) to enhance user productivity and efficiency through conversational and proactive experiences
scribd.com
. In practical terms, Now Assist helps users get quicker answers, better self-service, and intelligent recommendations within ServiceNow workflows. These AI-driven capabilities are delivered via specialized “Now Assist for X” applications that embed generative AI into different product areas (ITSM, CSM, HR, etc.)
scribd.com
scribd.com
. All Now Assist features are accessible through a unified Now Assist Admin Console, which provides a centralized interface for admins to set up, configure, and monitor Now Assist applications and features
scribd.com
. Now Assist Panel: A core component of the user experience is the Now Assist panel – a conversational AI side-panel available in ServiceNow workspaces. The panel acts as a real-time AI assistant that can summarize records and chats, answer questions, and suggest actions. For example, in Service Operations Workspace an agent can open the Now Assist panel to summarize an ongoing chat, an incident, or the resolution notes of a ticket, instantly getting the context without reading through the entire history
scribd.com
. In a CSM workspace, the same panel can summarize customer case details or chat interactions, helping the agent quickly grasp the issue
scribd.com
. The panel also supports a global AI search skill – users can ask questions in natural language, and Now Assist will retrieve answers from knowledge base articles (with source citations), or seamlessly fall back to a broader search if needed
scribd.com
scribd.com
. Overall, the Now Assist panel provides a conversational interface in context, whether the user is handling an IT incident, a customer case, or another workflow. Now Assist Skills: Under the hood, Now Assist delivers various generative AI “skills” tailored to different tasks and workflows
scribd.com
. These skills are discrete AI capabilities – for example, drafting a summary, generating a reply, or creating code – invoked by the system or on-demand. Key skills include case/incident summarization, chat summarization, resolution note generation, story generation, and even code generation
scribd.com
. The platform uses the context of your ServiceNow records and conversations as input to the LLM. Now Assist’s built-in skills cover a wide range of use cases out-of-the-box, and ServiceNow also provides a Now Assist Skill Kit for developing custom AI skills if needed (beyond the scope of Yokohama release notes).
Key Generative AI Capabilities in Now Assist
Now Assist introduces multiple AI-driven capabilities across ServiceNow’s product lines. The following are the core functionalities that Now Assist provides, as reflected in the Yokohama release:
Summarization of Records and Chats: Now Assist can automatically summarize the content of tickets, cases, and chats. For example, agents can view a concise summary of an incident or change request via an intuitive summarization UI
scribd.com
, or get a customer case summary and interaction recap in the workspace. Chat transcripts (e.g. live agent chats or virtual agent conversations) can be summarized in real time as well, allowing quick comprehension of long dialogues. This helps agents and managers quickly grasp the situation without reading lengthy logs.
Resolution Notes Generation: The platform can generate draft resolution notes for incidents, cases, or other tasks using GenAI. After an issue is resolved, Now Assist uses the conversation and activity history to propose a summary of the resolution. Users can then edit or refine this note as needed. In Yokohama, Now Assist even allows the agent to shorten or elaborate the AI-generated resolution content via a context menu for fine-tuning
scribd.com
. This capability ensures that knowledge is captured consistently and saves agents time in writing up resolutions.
Email and Chat Reply Recommendations: Now Assist helps craft responses to customers and users. It can automatically generate recommended email reply drafts for agents, based on the context of an incoming inquiry
scribd.com
. Agents see an AI-suggested email response which they can review, edit, and send, speeding up communications. Similarly, for live chats (or messaging channels like Slack and Teams), Now Assist can suggest conversational replies. Yokohama introduced enhancements to make these AI replies more natural – the Now Assist context menu lets agents adjust the tone of the suggested reply (e.g. casual, formal, sympathetic) to best fit the situation
scribd.com
scribd.com
. This ensures AI recommendations align with the company’s communication style. (For Microsoft Teams or Slack integrations, Now Assist can also generate “synthesized responses” to user queries in those channels
scribd.com
.)
Knowledge and Self-Service “Genius” Results: Now Assist improves self-service by intelligently surfacing solutions to users before they create a new ticket. When an end-user is filling out a case or incident form on a portal, Now Assist can use genius results (AI-powered search) to suggest relevant knowledge articles or answers that might solve their problem
scribd.com
. This helps deflect tickets by guiding users to self-help. If the user is interacting via Virtual Agent, Now Assist’s integration can also provide answers from the knowledge base conversationally. In the Yokohama release, the Now Assist panel’s Q&A ability (mentioned above) is a manifestation of this, where it displays knowledge article answers with source info when a question is asked
scribd.com
. Overall, Now Assist empowers users to find solutions more effectively, reducing the load on support teams.
LLM-Powered Scheduling Assistant: In Customer workflows, Now Assist enhances appointment scheduling. Yokohama introduced Now LLM scheduling assistance in Virtual Agent for booking appointments
scribd.com
. This makes scheduling interactions (like booking or rescheduling an appointment in a field service or customer service context) more conversational and intelligent. Users can simply ask to reschedule or find an available slot, and the AI handles the dialogue to update the appointment, improving user experience with natural language processing.
Intelligent Change Risk Analysis (ITSM): Now Assist extends to IT Service Management with skills that analyze and explain change risks. It can evaluate a Change Request and provide a risk explanation using past similar changes and incidents as context. Yokohama allows admins to copy and customize the Change Risk Explanation skill to include additional input fields (like specific risk assessment values or custom change attributes)
scribd.com
scribd.com
. This means Now Assist can be tuned to an organization’s change management data, giving a quick AI-generated summary of the risk of a change. Additionally, Now Assist for ITSM can refine the level of detail in its risk explanation (shortening or lengthening the response on demand)
scribd.com
.
AI-Assisted Development (App Engine): For developers and low-code creators, Now Assist for Creator brings generative AI to App Engine. One major feature is application generation – users (with appropriate roles) can describe an app they want, and Now Assist’s “app generation” skill will create the base application (data structure, interfaces, etc.) for them
scribd.com
. Yokohama expanded this by allowing even non-admin users (with a guided App Creator role) to use this feature and by improving the preview of the AI-generated app before creation
scribd.com
scribd.com
. Another development aid is Now Assist for Code, which helps in writing and editing code. In Yokohama, a Quick Actions button in Studio’s script editor lets developers invoke AI to suggest code changes or add comments to code
scribd.com
. There’s also an App Summary generation skill that can summarize an entire application’s purpose and functionality in text
scribd.com
 – useful for documentation or understanding someone else’s app. These capabilities accelerate development by leveraging AI for boilerplate code, documentation, and coding suggestions.
Workflow and Project Enhancements: Now Assist also aids in project and portfolio management. For example, Now Assist for SPM (Strategic Portfolio Management) can generate project artifacts and insights. A highlight in Yokohama is the ability to break down epics into user stories using an AI skill – Now Assist analyzes an epic’s details and recommends a set of stories (which the product manager can accept or refine) to accelerate Agile planning
scribd.com
scribd.com
. It also can summarize feedback or content in project docs and even create demands through conversations
scribd.com
, giving portfolio managers quick insights and automating some planning tasks. Similarly in HR, Now Assist for HRSD can automate aspects of employee journeys – for instance, generating a Journey Accelerator plan (a sequence of onboarding/offboarding tasks) from a simple prompt in the Journey Designer, rather than manually building out the plan
scribd.com
scribd.com
. Domain-specific AI assistance like this helps specialists (project managers, HR managers, etc.) work faster and make informed decisions with AI-generated suggestions.
Industry-Specific AI Use Cases: ServiceNow has extended Now Assist into industry solutions. For example, Now Assist for TMT (Telecommunications, Media, & Technology) uses generative AI in telecom service workflows. It can summarize complex onboarding cases, engagements, and touchpoints, produce enhanced summaries of test results (including metrics and context), and even aggregate insights from similar cases to draft knowledge articles
scribd.com
scribd.com
. Another noteworthy feature is dynamic prompt configuration – admins can configure how the AI summarizes cases to ensure important details for that industry are always included
scribd.com
. While these industry apps are specialized, they underscore Now Assist’s versatility: the framework can generate content like resolution notes, knowledge articles, or case summaries tailored to specific domains (IT, HR, customer service, software engineering, telecom, and more).
Now Assist Applications by Domain
Each Now Assist for X module is a separate application/plugin focusing on a particular ServiceNow product or workflow, but all share the common goal of injecting generative AI into that area. Below is a rundown of the primary Now Assist modules available as of the Yokohama release, and the capabilities they bring:
Now Assist for IT Service Management (ITSM): Brings GenAI to incident, problem, and change processes. Includes incident summary and change summary generation in Agent Workspace, AI-driven change risk explanations, email reply suggestions for incidents/problems, and portal deflection (genius search results for IT issues)
scribd.com
scribd.com
. For ITSM Virtual Agent, Now Assist provides pre-built topics that let users track task status or get help, using LLM to interpret requests (e.g. “What’s the status of my ticket?”)
scribd.com
. Now Assist for ITSM is delivered via the ServiceNow Store and was enhanced in Yokohama to include a new summarization UI and refined change risk and email recommendation features
scribd.com
scribd.com
.
Now Assist for Customer Service Management (CSM): Introduces AI assistance in customer support workflows. It can summarize customer chat conversations and case details, generate case resolution notes, and recommend next-best actions to agents. Yokohama highlights include an AI-powered scheduling assistant in Virtual Agent for appointment booking, genius results on the case intake form to suggest knowledge solutions to customers, and the ability to ask questions in the Now Assist panel to get knowledge base answers
scribd.com
scribd.com
. There were also improvements to AI reply recommendations (tone adjustments and multi-channel support) and better admin controls for enabling/disabling these features on specific channels
scribd.com
scribd.com
. Now Assist for CSM is a store app that brings generative AI to customer service agents and was first introduced in late Vancouver and fully supported in Yokohama.
Now Assist for HR Service Delivery (HRSD): Empowers HR agents with AI to handle employee inquiries. It can summarize HR cases and HR chat sessions, draft resolution notes for HR cases, and help propose solutions faster by understanding the context of HR issues
scribd.com
. In Yokohama, new HR Virtual Agent topics backed by Now Assist can create or update records in an HCM system via simple employee requests (e.g. “I need to update my address” triggers the appropriate workflow)
scribd.com
. A unique feature in HRSD is the Knowledge Graph integration – admins can build a personalized knowledge model (ontology) for HR, and Now Assist will use that to give more context-aware answers to employee questions
scribd.com
scribd.com
. Additionally, Now Assist can assist in creating Journey Accelerator plans (for onboarding, offboarding, etc.) from prompts, automating what used to be manual configuration
scribd.com
scribd.com
. This module, like others, is available via the Store for HRSD customers.
Now Assist for Strategic Portfolio Management (SPM): Applies generative AI to project and portfolio management. Key capabilities include summarizing feedback or content in project documents, generating concise project status summaries, assisting in demand creation through chat, and breaking down work. Yokohama specifically added the Epic-to-Story generation feature in Enterprise Agile Planning – Now Assist analyzes an epic and suggests user stories, which can be reviewed and then automatically created, accelerating backlog grooming
scribd.com
scribd.com
. It also provides quick insights into projects (for example, summarizing project updates or lessons learned documents). Now Assist for SPM requires the SPM Pro license and is installable from the Store
scribd.com
.
Now Assist for Creator (App Engine): Focused on accelerating application development on the Now Platform. It offers the App Generation skill to build new apps from a textual description, as well as Analytics Generation (for building dashboard analytics via natural language queries). Yokohama’s release improved app generation by enabling guided App Creator roles and enhancing the preview experience
scribd.com
. Another aspect is Now Assist for Code, which aids in script writing and editing – developers can get code suggestions or have the AI explain sections of code. The module also introduced an App Summary skill to instantly document an application’s functionality
scribd.com
. Additionally, in the RPA Hub (Robotic Process Automation), Now Assist can help create and refine RPA bot workflows using plain language instructions
scribd.com
scribd.com
. All these capabilities target faster development cycles. Now Assist for Creator is distributed via the Store and works in conjunction with App Engine Studio, Integration Hub, and other dev tools.
Other Now Assist Modules: ServiceNow has extended Now Assist to many other domains, often available as Store apps for customers of those products. For example, Now Assist for IT Operations Management (ITOM) can potentially summarize incidents or changes in IT infrastructure and help with automated remediation suggestions. Now Assist for Integrated Risk Management (IRM) can summarize risk issues or vendor risk assessments (e.g., generating an issue summary to speed up risk triage). Now Assist for Field Service Management (FSM) likely assists dispatchers and technicians by summarizing work order details or generating closure notes. Niche areas like Legal Service Delivery (LSD) and Public Sector Digital Services (PSDS) also have Now Assist integrations
scribd.com
scribd.com
, indicating ServiceNow’s push to embed AI in industry-specific workflows. These modules typically provide similar capabilities – summarizing records, recommending next steps, and generating relevant content – but tuned to the context of those applications (e.g., legal case summaries, public sector service case notes, etc.). All Now Assist modules follow the same model of plugin delivery via the Store and require the respective product’s top-tier license.
Entitlement, Installation, and Configuration
Licensing Requirements: Now Assist features are entitlement-controlled. Each Now Assist module requires the customer to have the “Pro+” or Enterprise tier license for the corresponding product. For example, Now Assist for ITSM requires an ITSM Pro Plus or Enterprise Plus license to use
scribd.com
. Similarly, Now Assist for CSM is only available to CSM Professional Plus or Enterprise customers
scribd.com
. (In other words, generative AI is an add-on to the highest-level SKUs of ITSM, CSM, HRSD, SPM, etc.) If a customer does not have the required license, the Now Assist plugin for that area will not be entitled for installation. Administrators should verify they have the appropriate subscriptions (e.g. ITSM Pro+) before attempting to enable Now Assist for that product. Plugin Installation: All Now Assist capabilities are delivered as separate plugins (applications) via the ServiceNow Store. They are not enabled by default in the base platform and must be retrieved and installed. ServiceNow has made the installation process straightforward through the Now Assist Admin Console: an admin can navigate to Now Assist Admin > Settings > Plugins, and from there click “Get plugins” on the desired Now Assist feature to fetch it from the Store
scribd.com
. (This assumes the instance is online and authorized to download Store apps.) Alternatively, one can manually install by visiting the ServiceNow Store listings for Now Assist for ITSM, Now Assist for CSM, etc., and requesting the app. Once approved and installed, each Now Assist plugin will typically bring its own components (AI skills, UI elements, roles, etc.) into the instance. For example, installing Now Assist for ITSM will activate the “ITSM GenAI” plugin (plugin ID: com.sn.itsm.gen.ai) which adds the Now Assist panel in Service Operations Workspace and related AI capabilities
scribd.com
. ServiceNow usually delivers updates to these AI features through Store releases (outside of family upgrades), so admins should monitor the Store for any patches or new versions of Now Assist apps. Configuration and Setup: After installation, there are a few steps to get Now Assist fully operational:
Generative AI Connection: Now Assist relies on ServiceNow’s Generative AI Controller as the integration layer to third-party LLM services. This controller plugin is automatically installed as a dependency for Now Assist features. The admin needs to configure it with credentials for an LLM provider (such as OpenAI or Microsoft Azure OpenAI) unless ServiceNow’s own managed LLM (“Now LLM”) is used. The Generative AI Controller acts as the broker between ServiceNow and the large language model APIs
servicenow.com
, enabling secure calls to generate text. In practice, setting this up involves installing the OpenAI or Azure OpenAI integration spokes (which the controller can do) and entering API keys or connection information in the Generative AI Controller settings
reddit.com
. Once configured, the Now Assist features will leverage this connection to send prompts to the LLM and receive completions. (The Yokohama documentation and community resources note that the controller supports multiple providers, including OpenAI, Azure OpenAI, and Google’s PaLM/Gemini models
plat4mation.com
, so organizations can choose their preferred AI backend.)
Now Assist Admin Console: With the plugins installed and AI connectivity in place, the primary interface to enable/disable and tune Now Assist is the Now Assist Admin Console. This console (accessible to admins with the proper roles, e.g. now_assist_admin) allows configuration of each AI skill and feature. For instance, admins can turn on or off features like email recommendations or chat summaries per application, set which channels (portal, workspace, mobile) they are active in, and assign roles for who can use the Now Assist panel
scribd.com
. The console also provides monitoring capabilities – for example, viewing usage statistics of Now Assist (how often AI suggestions are used) and managing any feedback loops (such as improving prompts based on user feedback). Essentially, it’s the control center for all Now Assist functionality across the instance
scribd.com
.
Security and Data Handling: (While not explicitly detailed in the release notes, it’s worth noting as part of setup.) Because Now Assist involves sending potentially sensitive data to an LLM, ServiceNow has an AI Acceptable Use Policy
servicenow.com
 and admins should ensure compliance (e.g., deciding which fields or data are allowed in prompts). The Generative AI Controller offers options to mask or exclude certain data from being sent to the LLM. During configuration, admins should review these settings to align with their company’s data privacy requirements. All Now Assist plugins abide by the platform’s role-based access, so only authorized users (like fulfiller agents or specific roles) can invoke the AI features.
Activation Summary: In summary, to go from zero to full use of Now Assist, an admin must: 1) obtain the necessary license entitlements (Pro/Enterprise for relevant products), 2) install the Now Assist plugins from the ServiceNow Store (either via the Store or the in-platform plugin installer)
scribd.com
, 3) configure the Generative AI Controller with an AI provider and verify the connection, and 4) use the Now Assist Admin Console to enable desired features and adjust settings. Once this is done, end-users (agents, developers, etc.) will see the “Now Assist” panel and options appear in their Workspaces or Studio, and they can begin leveraging generative AI assistance in their daily tasks. With Yokohama’s release, Now Assist has matured into a robust toolkit, and following these steps ensures your instance is fully equipped to use AI safely and effectively. Sources: The information above was synthesized from the official ServiceNow Yokohama release notes and documentation for Now Assist
scribd.com
scribd.com
scribd.com
scribd.com
scribd.com
scribd.com
servicenow.com
, which describe the new capabilities, installation procedures, and requirements in detail.





Sources






You said:
Understood. I’ll conduct an in-depth review of the Now Assist capabilities introduced and enhanced in the ServiceNow Yokohama release, along with foundational features and setup requirements.

I will provide a comprehensive set of concise declarative statements covering all functional areas, including Virtual Agent, Agent Assist, Search, Case Summarization, and more. A dedicated section will detail plugin entitlements, installation, and configuration necessary for initial setup and operational readiness.

I’ll let you know once the full set of statements is ready for your review.


# Now Assist in ServiceNow’s Yokohama Release – Overview and Key Capabilities

## Now Assist Overview

**Now Assist** is ServiceNow’s family of generative AI features integrated across the Now Platform. It uses large language models (LLMs) to enhance user productivity and efficiency through conversational and proactive experiences. In practical terms, Now Assist helps users get quicker answers, better self-service, and intelligent recommendations within ServiceNow workflows. These AI-driven capabilities are delivered via specialized “Now Assist for *X*” applications that embed generative AI into different product areas (ITSM, CSM, HR, etc.). All Now Assist features are accessible through a unified **Now Assist Admin Console**, which provides a centralized interface for admins to **set up, configure, and monitor** Now Assist applications and features.

**Now Assist Panel:** A core component of the user experience is the Now Assist panel – a conversational AI side-panel available in ServiceNow workspaces. The panel acts as a real-time AI assistant that can summarize records and chats, answer questions, and suggest actions. For example, in Service Operations Workspace an agent can open the Now Assist panel to **summarize an ongoing chat, an incident, or the resolution notes** of a ticket, instantly getting the context without reading through the entire history. In a CSM workspace, the same panel can summarize customer case details or chat interactions, helping the agent quickly grasp the issue. The panel also supports a **global AI search** skill – users can ask questions in natural language, and Now Assist will retrieve answers from knowledge base articles (with source citations), or seamlessly fall back to a broader search if needed. Overall, the Now Assist panel provides a conversational interface in context, whether the user is handling an IT incident, a customer case, or another workflow.

**Now Assist Skills:** Under the hood, Now Assist delivers various **generative AI “skills”** tailored to different tasks and workflows. These skills are discrete AI capabilities – for example, drafting a summary, generating a reply, or creating code – invoked by the system or on-demand. Key skills include **case/incident summarization, chat summarization, resolution note generation, story generation**, and even **code generation**. The platform uses the context of your ServiceNow records and conversations as input to the LLM. Now Assist’s built-in skills cover a wide range of use cases out-of-the-box, and ServiceNow also provides a **Now Assist Skill Kit** for developing custom AI skills if needed (beyond the scope of Yokohama release notes).

## Key Generative AI Capabilities in Now Assist

Now Assist introduces multiple AI-driven capabilities across ServiceNow’s product lines. The following are the **core functionalities** that Now Assist provides, as reflected in the Yokohama release:

* **Summarization of Records and Chats:** Now Assist can automatically **summarize the content of tickets, cases, and chats**. For example, agents can view a concise summary of an incident or change request via an intuitive summarization UI, or get a customer case summary and interaction recap in the workspace. Chat transcripts (e.g. live agent chats or virtual agent conversations) can be summarized in real time as well, allowing quick comprehension of long dialogues. This helps agents and managers quickly grasp the situation without reading lengthy logs.

* **Resolution Notes Generation:** The platform can **generate draft resolution notes** for incidents, cases, or other tasks using GenAI. After an issue is resolved, Now Assist uses the conversation and activity history to propose a summary of the resolution. Users can then edit or refine this note as needed. In Yokohama, Now Assist even allows the agent to **shorten or elaborate the AI-generated resolution content via a context menu** for fine-tuning. This capability ensures that knowledge is captured consistently and saves agents time in writing up resolutions.

* **Email and Chat Reply Recommendations:** Now Assist helps craft responses to customers and users. It can **automatically generate recommended email reply drafts** for agents, based on the context of an incoming inquiry. Agents see an AI-suggested email response which they can review, edit, and send, speeding up communications. Similarly, for live chats (or messaging channels like Slack and Teams), Now Assist can suggest conversational replies. Yokohama introduced enhancements to make these AI replies more natural – the Now Assist context menu lets agents **adjust the tone** of the suggested reply (e.g. casual, formal, sympathetic) to best fit the situation. This ensures AI recommendations align with the company’s communication style. (For Microsoft Teams or Slack integrations, Now Assist can also generate “synthesized responses” to user queries in those channels.)

* **Knowledge and Self-Service “Genius” Results:** Now Assist improves self-service by intelligently surfacing solutions to users *before* they create a new ticket. When an end-user is filling out a case or incident form on a portal, Now Assist can use **genius results (AI-powered search)** to suggest relevant knowledge articles or answers that might solve their problem. This helps deflect tickets by guiding users to self-help. If the user is interacting via Virtual Agent, Now Assist’s integration can also provide answers from the knowledge base conversationally. In the Yokohama release, the Now Assist panel’s Q\&A ability (mentioned above) is a manifestation of this, where it displays knowledge article answers with source info when a question is asked. Overall, Now Assist empowers users to find solutions more effectively, reducing the load on support teams.

* **LLM-Powered Scheduling Assistant:** In Customer workflows, Now Assist enhances appointment scheduling. Yokohama introduced **Now LLM scheduling assistance** in Virtual Agent for booking appointments. This makes scheduling interactions (like booking or rescheduling an appointment in a field service or customer service context) more conversational and intelligent. Users can simply ask to reschedule or find an available slot, and the AI handles the dialogue to update the appointment, improving user experience with natural language processing.

* **Intelligent Change Risk Analysis (ITSM):** Now Assist extends to IT Service Management with skills that analyze and explain change risks. It can evaluate a Change Request and provide a **risk explanation** using past similar changes and incidents as context. Yokohama allows admins to **copy and customize the Change Risk Explanation skill** to include additional input fields (like specific risk assessment values or custom change attributes). This means Now Assist can be tuned to an organization’s change management data, giving a quick AI-generated summary of the risk of a change. Additionally, Now Assist for ITSM can refine the level of detail in its risk explanation (shortening or lengthening the response on demand).

* **AI-Assisted Development (App Engine):** For developers and low-code creators, **Now Assist for Creator** brings generative AI to App Engine. One major feature is **application generation** – users (with appropriate roles) can describe an app they want, and Now Assist’s “app generation” skill will create the base application (data structure, interfaces, etc.) for them. Yokohama expanded this by allowing even non-admin users (with a guided App Creator role) to use this feature and by improving the preview of the AI-generated app before creation. Another development aid is **Now Assist for Code**, which helps in writing and editing code. In Yokohama, a **Quick Actions** button in Studio’s script editor lets developers invoke AI to suggest code changes or add comments to code. There’s also an **App Summary generation** skill that can summarize an entire application’s purpose and functionality in text – useful for documentation or understanding someone else’s app. These capabilities accelerate development by leveraging AI for boilerplate code, documentation, and coding suggestions.

* **Workflow and Project Enhancements:** Now Assist also aids in project and portfolio management. For example, **Now Assist for SPM (Strategic Portfolio Management)** can generate project artifacts and insights. A highlight in Yokohama is the ability to **break down epics into user stories** using an AI skill – Now Assist analyzes an epic’s details and recommends a set of stories (which the product manager can accept or refine) to accelerate Agile planning. It also can summarize feedback or content in project docs and even create demands through conversations, giving portfolio managers quick insights and automating some planning tasks. Similarly in HR, **Now Assist for HRSD** can automate aspects of employee journeys – for instance, generating a Journey Accelerator plan (a sequence of onboarding/offboarding tasks) from a simple prompt in the Journey Designer, rather than manually building out the plan. Domain-specific AI assistance like this helps specialists (project managers, HR managers, etc.) work faster and make informed decisions with AI-generated suggestions.

* **Industry-Specific AI Use Cases:** ServiceNow has extended Now Assist into industry solutions. For example, **Now Assist for TMT (Telecommunications, Media, & Technology)** uses generative AI in telecom service workflows. It can **summarize complex onboarding cases, engagements, and touchpoints**, produce enhanced summaries of test results (including metrics and context), and even aggregate insights from similar cases to draft knowledge articles. Another noteworthy feature is **dynamic prompt configuration** – admins can configure how the AI summarizes cases to ensure important details for that industry are always included. While these industry apps are specialized, they underscore Now Assist’s versatility: the framework can generate content like resolution notes, knowledge articles, or case summaries tailored to specific domains (IT, HR, customer service, software engineering, telecom, and more).

## Now Assist Applications by Domain

Each **Now Assist for *X*** module is a separate application/plugin focusing on a particular ServiceNow product or workflow, but all share the common goal of injecting generative AI into that area. Below is a rundown of the primary Now Assist modules available as of the Yokohama release, and the capabilities they bring:

* **Now Assist for IT Service Management (ITSM):** Brings GenAI to incident, problem, and change processes. Includes **incident summary** and **change summary** generation in Agent Workspace, AI-driven **change risk explanations**, **email reply suggestions** for incidents/problems, and **portal deflection** (genius search results for IT issues). For ITSM Virtual Agent, Now Assist provides pre-built topics that let users track task status or get help, using LLM to interpret requests (e.g. “What’s the status of my ticket?”). *Now Assist for ITSM* is delivered via the ServiceNow Store and was enhanced in Yokohama to include a new summarization UI and refined change risk and email recommendation features.

* **Now Assist for Customer Service Management (CSM):** Introduces AI assistance in customer support workflows. It can **summarize customer chat conversations and case details**, generate **case resolution notes**, and recommend next-best actions to agents. Yokohama highlights include an **AI-powered scheduling assistant** in Virtual Agent for appointment booking, **genius results on the case intake form** to suggest knowledge solutions to customers, and the ability to **ask questions in the Now Assist panel** to get knowledge base answers. There were also improvements to **AI reply recommendations** (tone adjustments and multi-channel support) and better admin controls for enabling/disabling these features on specific channels. *Now Assist for CSM* is a store app that brings generative AI to customer service agents and was first introduced in late Vancouver and fully supported in Yokohama.

* **Now Assist for HR Service Delivery (HRSD):** Empowers HR agents with AI to handle employee inquiries. It can **summarize HR cases and HR chat sessions**, draft **resolution notes for HR cases**, and help propose solutions faster by understanding the context of HR issues. In Yokohama, new **HR Virtual Agent topics** backed by Now Assist can create or update records in an HCM system via simple employee requests (e.g. “I need to update my address” triggers the appropriate workflow). A unique feature in HRSD is the **Knowledge Graph integration** – admins can build a personalized knowledge model (ontology) for HR, and Now Assist will use that to give more context-aware answers to employee questions. Additionally, Now Assist can assist in creating **Journey Accelerator** plans (for onboarding, offboarding, etc.) from prompts, automating what used to be manual configuration. This module, like others, is available via the Store for HRSD customers.

* **Now Assist for Strategic Portfolio Management (SPM):** Applies generative AI to project and portfolio management. Key capabilities include **summarizing feedback or content** in project documents, generating concise **project status summaries**, assisting in **demand creation** through chat, and breaking down work. Yokohama specifically added the **Epic-to-Story generation** feature in Enterprise Agile Planning – Now Assist analyzes an epic and suggests user stories, which can be reviewed and then automatically created, accelerating backlog grooming. It also provides quick insights into projects (for example, summarizing project updates or lessons learned documents). *Now Assist for SPM* requires the SPM Pro license and is installable from the Store.

* **Now Assist for Creator (App Engine):** Focused on accelerating application development on the Now Platform. It offers the **App Generation** skill to build new apps from a textual description, as well as **Analytics Generation** (for building dashboard analytics via natural language queries). Yokohama’s release improved app generation by enabling guided App Creator roles and enhancing the preview experience. Another aspect is **Now Assist for Code**, which aids in script writing and editing – developers can get code suggestions or have the AI explain sections of code. The module also introduced an **App Summary** skill to instantly document an application’s functionality. Additionally, in the RPA Hub (Robotic Process Automation), Now Assist can help create and refine **RPA bot workflows** using plain language instructions. All these capabilities target faster development cycles. *Now Assist for Creator* is distributed via the Store and works in conjunction with App Engine Studio, Integration Hub, and other dev tools.

* **Other Now Assist Modules:** ServiceNow has extended Now Assist to many other domains, often available as Store apps for customers of those products. For example, **Now Assist for IT Operations Management (ITOM)** can potentially summarize incidents or changes in IT infrastructure and help with automated remediation suggestions. **Now Assist for Integrated Risk Management (IRM)** can summarize risk issues or vendor risk assessments (e.g., generating an issue summary to speed up risk triage). **Now Assist for Field Service Management (FSM)** likely assists dispatchers and technicians by summarizing work order details or generating closure notes. Niche areas like **Legal Service Delivery (LSD)** and **Public Sector Digital Services (PSDS)** also have Now Assist integrations, indicating ServiceNow’s push to embed AI in industry-specific workflows. These modules typically provide similar capabilities – summarizing records, recommending next steps, and generating relevant content – but tuned to the context of those applications (e.g., legal case summaries, public sector service case notes, etc.). All Now Assist modules follow the same model of plugin delivery via the Store and require the respective product’s top-tier license.

## Entitlement, Installation, and Configuration

**Licensing Requirements:** Now Assist features are **entitlement-controlled**. Each Now Assist module requires the customer to have the **“Pro+” or Enterprise tier license** for the corresponding product. For example, *Now Assist for ITSM* requires an ITSM **Pro Plus or Enterprise Plus** license to use. Similarly, *Now Assist for CSM* is only available to CSM **Professional Plus or Enterprise** customers. (In other words, generative AI is an add-on to the highest-level SKUs of ITSM, CSM, HRSD, SPM, etc.) If a customer does not have the required license, the Now Assist plugin for that area will not be entitled for installation. Administrators should verify they have the appropriate subscriptions (e.g. ITSM Pro+) before attempting to enable Now Assist for that product.

**Plugin Installation:** All Now Assist capabilities are delivered as **separate plugins (applications) via the ServiceNow Store**. They are not enabled by default in the base platform and must be retrieved and installed. ServiceNow has made the installation process straightforward through the Now Assist Admin Console: an admin can navigate to **Now Assist Admin > Settings > Plugins**, and from there click “Get plugins” on the desired Now Assist feature to fetch it from the Store. (This assumes the instance is online and authorized to download Store apps.) Alternatively, one can manually install by visiting the ServiceNow Store listings for *Now Assist for ITSM*, *Now Assist for CSM*, etc., and requesting the app. Once approved and installed, each Now Assist plugin will typically bring its own components (AI skills, UI elements, roles, etc.) into the instance. For example, installing Now Assist for ITSM will activate the “ITSM GenAI” plugin (plugin ID: *com.sn.itsm.gen.ai*) which adds the Now Assist panel in Service Operations Workspace and related AI capabilities. ServiceNow usually delivers updates to these AI features through Store releases (outside of family upgrades), so admins should monitor the Store for any **patches or new versions** of Now Assist apps.

**Configuration and Setup:** After installation, there are a few steps to get Now Assist fully operational:

* **Generative AI Connection:** Now Assist relies on ServiceNow’s **Generative AI Controller** as the integration layer to third-party LLM services. This controller plugin is automatically installed as a dependency for Now Assist features. The admin needs to configure it with credentials for an LLM provider (such as OpenAI or Microsoft Azure OpenAI) unless ServiceNow’s own managed LLM (“Now LLM”) is used. The Generative AI Controller acts as the broker between ServiceNow and the large language model APIs, enabling secure calls to generate text. In practice, setting this up involves installing the **OpenAI or Azure OpenAI integration spokes** (which the controller can do) and entering API keys or connection information in the Generative AI Controller settings. Once configured, the Now Assist features will leverage this connection to send prompts to the LLM and receive completions. (The Yokohama documentation and community resources note that the controller supports multiple providers, including OpenAI, Azure OpenAI, and Google’s PaLM/Gemini models, so organizations can choose their preferred AI backend.)

* **Now Assist Admin Console:** With the plugins installed and AI connectivity in place, the primary interface to enable/disable and tune Now Assist is the **Now Assist Admin Console**. This console (accessible to admins with the proper roles, e.g. now_assist_admin) allows configuration of each AI skill and feature. For instance, admins can turn on or off features like email recommendations or chat summaries per application, set which channels (portal, workspace, mobile) they are active in, and assign roles for who can use the Now Assist panel. The console also provides monitoring capabilities – for example, viewing usage statistics of Now Assist (how often AI suggestions are used) and managing any feedback loops (such as improving prompts based on user feedback). Essentially, it’s the control center for all Now Assist functionality across the instance.

* **Security and Data Handling:** (While not explicitly detailed in the release notes, it’s worth noting as part of setup.) Because Now Assist involves sending potentially sensitive data to an LLM, ServiceNow has an **AI Acceptable Use Policy** and admins should ensure compliance (e.g., deciding which fields or data are allowed in prompts). The Generative AI Controller offers options to mask or exclude certain data from being sent to the LLM. During configuration, admins should review these settings to align with their company’s data privacy requirements. All Now Assist plugins abide by the platform’s role-based access, so only authorized users (like fulfiller agents or specific roles) can invoke the AI features.

**Activation Summary:** In summary, to go from zero to full use of Now Assist, an admin must: 1) obtain the necessary license entitlements (Pro/Enterprise for relevant products), 2) install the Now Assist plugins from the ServiceNow Store (either via the Store or the in-platform plugin installer), 3) configure the Generative AI Controller with an AI provider and verify the connection, and 4) use the Now Assist Admin Console to enable desired features and adjust settings. Once this is done, end-users (agents, developers, etc.) will see the **“Now Assist” panel and options** appear in their Workspaces or Studio, and they can begin leveraging generative AI assistance in their daily tasks. With Yokohama’s release, Now Assist has matured into a robust toolkit, and following these steps ensures your instance is fully equipped to use AI safely and effectively.

**Sources:** The information above was synthesized from the official ServiceNow Yokohama release notes and documentation for Now Assist, which describe the new capabilities, installation procedures, and requirements in detail.

1. Identify ambiguous statements
2. For each ambiguous statement make a determination if the ambiguity is owed to legitimate uncertainty or if the ambiguity is owed to a lack of depth of research
3. Perform additional research as needed, clarifying ambiguous statements as you are able to based on the deeper research
4. Identify over-used statements as these could indicate a lack of depth of research, we want to avoid over-fitting reasonable facts to a broad set of concepts and thereby missing an opportunity to enhance nuance with deeper research
5. Perform additional research as needed, clarifying over-used statements as you are able to based on the deeper research


ChatGPT said:

Thought for 1m 42s
Below is a focused “critique pass” on the draft — calling out places where wording is vague or over-general, deciding why that vagueness exists, and tightening the language where the Yokohama release notes give a firm answer.

1 · Ambiguous or Vague Statements and How to Fix Them
#	Original wording (excerpt)	Why it’s ambiguous	Legitimate uncertainty?	Clarified wording (with source)
1	“All Now Assist features are accessible through a unified Now Assist Admin Console.”	Sounds absolute; some skills are managed elsewhere (e.g., AI Search).	Research gap	“Most GenAI skills are surfaced in the Now Assist Admin console; AI Search and Dynamic Translation continue to use their own admin pages.” 
downloads.docs.servicenow.com
2	“Global AI search skill.”	Not an official label; the product names it Conversational Q&A in the Assist panel.	Research gap	“The side panel includes Conversational Q&A, which lets agents ask natural-language questions; answers come from AI Search with inline article citations.” 
scribd.com
3	“ServiceNow also provides a Now Assist Skill Kit … (beyond the scope).”	Presence/GA status unclear.	Research gap	“The Now Assist Skill Kit is GA in Yokohama; admins can duplicate out-of-box skills, swap prompts/providers, and add Retrieval-Augmented Generation (RAG) tools.” 
scribd.com
4	“Now Assist for ITOM, IRM, FSM can potentially …”	“Potentially” implies guesswork.	Research gap	“Yokohama ships certified store apps for Now Assist for ITOM, IRM, FSM, LSD, PSDS, SAM, SGC, SLO, SPO, FSO, EA, CMDB, APO, WSD, each delivering the familiar summarise/resolve/knowledge skills for its data models.” 
scribd.com
5	“Now Assist uses multiple LLM providers (OpenAI, Azure OpenAI, Google Gemini).”	Exact list not in notes; TMT doc says only Now LLM.	Legitimately variable (depends on skill)	Clarify per-app: “Most skills run through Generative AI Controller, which supports any connected provider; some industry apps (e.g., TMT) are pinned to ServiceNow-hosted Now LLM Service.” 
downloads.docs.servicenow.com
scribd.com
6	“Licensing: Pro+ or Enterprise.”	“Pro+” is informal; official strings are Pro Plus / Enterprise Plus.	Research gap	“Each Assist app requires the top-tier SKU for its product (ITSM Pro Plus or Enterprise Plus, CSM Professional Plus, SPM Pro Plus, etc.).” 
servicenow.com
7	“Generative AI Controller installs OpenAI or Azure spokes (which the controller can do) …”	Over-broad; controller only invokes spokes the admin installs.	Research gap	“Admins install the Generative AI Controller (auto-dependency of any Assist app) and then add one or more LLM spokes (OpenAI, Azure OpenAI, or other certified providers) before enabling skills.” 
scribd.com
8	“Knowledge and self-service ‘Genius’ results.”	‘Genius results’ is internal jargon; official term in notes is AI Search recommendations.	Research gap	“Portal intake forms surface AI Search recommendations (formerly ‘genius results’) that deflect tickets by showing relevant KB articles in real time.” 
scribd.com
9	“LLM-powered scheduling assistant” (no module named).	Readers may ask “where exactly?”	Legitimate (feature lives inside Virtual Agent for CSM/FSM)	Add: “Virtual Agent topics in Now Assist for CSM & FSM include an LLM-driven scheduling dialog for new or rescheduled appointments.” 
scribd.com

2 · Over-used or Blanket Phrases to Re-examine
“Improve productivity and efficiency by delivering better self-service, recommending actions, and providing answers.”
Repeated verbatim across many modules.

Fix: Reference the specific efficiency win per module, e.g., “reduce first-response time for customer chats by summarising 100-line transcripts into three bullet points.”

“Summarize X, generate resolution notes, and recommend next steps.”
Appears in nearly every module paragraph.

Fix: Spell out the domain twist:
ITOM: “summarise alerts into incident context and propose auto-remediation playbooks.”
IRM: “condense risk assessments and draft mitigation plans keyed to policy gaps.”

3 · Next-Step Research Done
Confirmed full list of Assist store apps and their licensing tiers. 
scribd.com
servicenow.com

Verified Skill Kit GA and ability to copy/extend skills. 
scribd.com

Located admin-console instructions and per-skill customisation flow. 
downloads.docs.servicenow.com

Identified official naming for Conversational Q&A, AI Search recommendations, and “Pro Plus/Enterprise Plus.” 
scribd.com
servicenow.com

These clarifications can now be folded back into the master document to eliminate hand-waving and tighten domain wording.












Sources
You said:
Proceed with producing a comprehensive final set of declarative statements


ChatGPT said:

Thought for 48 seconds
Now Assist — Declarative Knowledge Base (ServiceNow Yokohama release)
1 · Platform-wide fundamentals
Now Assist = the GenAI layer of the Now Platform. It wraps domain-specific skills (summarise, draft, recommend, generate) around ServiceNow records and conversations.
linkedin.com

Agents access GenAI through the Now Assist panel (side-panel in all workspaces) or via in-form action menus; both surface skills such as Conversational Q&A, summarisation, and reply drafting.
linkedin.com

Now Assist Admin Console centralises enable/disable switches, usage analytics, and skill-level prompt tuning for every installed Assist app.
plat4mation.com

A common Generative AI Controller plugin brokers calls to OpenAI, Azure OpenAI, Google Gemini, or ServiceNow-hosted “Now LLM Service”. Controller installs matching spokes (e.g., OpenAI Spoke).
store.servicenow.com
store.servicenow.com

AI Guardian enforces content filtering, jailbreak prevention, and sensitive-topic redirection for all GenAI traffic.
plat4mation.com

Skill Kit (GA in Yokohama) lets admins clone or extend any out-of-box skill, edit its prompt, swap LLM provider, or attach Retrieval-Augmented Generation sources.
plat4mation.com

New accessibility boosts: voice-enabled input to the panel and multi-turn conversational memory.
linkedin.com

2 · Cross-domain core skills
Record & chat summarisation — condenses tickets, cases, or transcripts into key bullet points on demand.

Resolution/closure note generation — drafts structured work notes after completion; agents can “shorten” or “elaborate” via context menu.
plat4mation.com

Email / chat reply suggestions — AI drafts a response; agents can swap tone (formal, casual, empathetic).
linkedin.com

Conversational Q&A — natural-language questions answered with AI Search, returning cited KB articles.
linkedin.com

Knowledge & portal deflection — AI Search recommendations (“Genius Results”) appear while users fill record producers, deflecting incidents.

3 · Module-specific assist apps (Store plugins)
Assist app	Yokohama highlights	Plugin ID / SKU	Notes
ITSM	Change-risk explanation (customisable inputs); incident & change summaries; email drafts; VA ticket-status topics.	sn_itsm_gen_ai	Needs ITSM Pro Plus or Enterprise Plus license.
store.servicenow.com
CSM	AI scheduling dialog in Virtual Agent; case & chat summaries; intake-form AI Search; tone-aware reply suggestions.	sn_csm_gen_ai	Requires CSM Professional Plus / Enterprise.
HRSD	HR case & chat summaries; Journey Accelerator plan generation; HR Knowledge Graph-aware answers; VA HCM record update topics.	sn_hrsd_gen_ai	Needs HRSD Enterprise Plus.
SPM	Epic-to-Story generation; demand creation via chat; project status summariser.	sn_spm_gen_ai	Needs SPM Pro Plus.
erp.today
Creator / App Engine	Text-to-App generation (preview before create); App-summary skill; code explanations & quick actions; ATF-test and RPA-bot generation.	sn_creator_gen_ai	Works with App Engine Pro / Enterprise.
inmorphis.com
ITOM, IRM, FSM, TMT, LSD, PSDS, SAM, etc.	Standard summarise / resolve / recommend skills wired to domain tables; some industry apps pin to ServiceNow-hosted Now LLM for compliance.	individual IDs	All require top-tier product SKUs.

4 · Licensing & entitlement rules
Each Assist plugin is gated by the product’s Pro Plus or Enterprise Plus subscription. Without that license the Store listing is not entitled.
store.servicenow.com

Assist usage consumes “Assists” (metered GenAI transactions) pooled at the account level; partner store apps may draw from the same pool.
store.servicenow.com

5 · Zero-to-live installation checklist
Verify license for target product (e.g., ITSM Pro Plus).

In the instance, go to Now Assist Admin → Plugins → Get plugins, install the desired Now Assist for X app (auto-installs Generative AI Controller).
store.servicenow.com

Open Generative AI Controller → add at least one LLM spoke (OpenAI, Azure OpenAI, Gemini) and enter API credentials or select Now LLM tenant.
store.servicenow.com
plat4mation.com

In Now Assist Admin Console, enable skills per channel (workspace, portal, mobile), assign roles, and review default prompts.
plat4mation.com

Optional hardening:

In AI Guardian, enable corporate policy pack (toxicity, privacy) and map sensitive columns for masking.
plat4mation.com

Under Overflow processing, choose on-premise only or allow cloud burst to Azure.
plat4mation.com

Validate: open a workspace record, launch the Now Assist panel, run “Summarise”; confirm output, tone-switch, and citation links.

6 · Governance & extensibility
Skill tuning: clone any skill → edit system prompt, add field selectors, or attach an AI Search index for RAG responses.

Prompt safety: AI Guardian monitors both user and system prompts; blocked content redirects to a safe Virtual Agent topic.

Data sovereignty: Organisations may opt out of cross-region “cloud bursting” when prompts exceed on-prem token limits.
plat4mation.com

Observability: Usage dashboards show per-skill adoption, success rate, and average assists consumed; exportable for charge-back. (Exposed in Admin Console.)

7 · Yokohama-exclusive upgrades at a glance
Upgrade	Impact	Where visible
Voice input to panel	Hands-free interactions for field/mobile agents.	All workspaces.
linkedin.com
Multi-turn conversational memory	Follow-up questions keep context.	Conversational Q&A.
linkedin.com
Tone selector & length control	Faster, brand-aligned replies/resolution notes.	Email/chat drafts, summarise context menu.
plat4mation.com
Epic-to-Story generator	Cuts backlog-grooming time.	SPM EAP board.
erp.today
ATF test & RPA bot generation	Automates developer scaffolding.	App Engine Studio / RPA Hub.
inmorphis.com

These statements establish a complete, unambiguous reference for future automation, testing, and governance work involving Now Assist in the Yokohama release.












Sources
You said:
Write a step by step plan to enable Now Assist in Yokohama, focus should be on a post-Yokohama upgrade deployment plan for Now Assist so that we can comprehensively utilize the full feature set

Write a step by step demo script that a human can follow to fully explore the capabilities of Now Assist Yokohama release


ChatGPT said:

Thought for 15 seconds
Post-Upgrade Deployment Plan & Demo Script — Now Assist (Yokohama release)

Part A · Deployment Plan (from fresh Yokohama instance to full production use)
Phase	Action	Key checks / outputs
0. Prep	- Confirm the instance is on Yokohama or later and that a non-prod clone is available for initial enablement.
- Gather “Pro Plus / Enterprise Plus” SKUs for each product you plan to pilot (e.g., ITSM Pro Plus, CSM Professional Plus). 
scribd.com
Upgrade complete; licenses present in the “Subscription Management” page.
1. Core plugins	In the non-prod instance go to Plugins › Find “Now Assist Admin Console” › Install. This auto-installs the Generative AI Controller dependency. 
servicenow.com
store.servicenow.com
sn_nowassist_admin and sn_genai_controller plugins show Active.
2. Domain apps	From Now Assist Admin Console › Plugins tab › Get plugins, install each required Assist app (ITSM, CSM, HRSD, etc.). 
servicenow.com
store.servicenow.com
Each sn_<product>_gen_ai plugin active.
3. LLM connectivity	In Generative AI Controller › Connections, select a provider:
• Now LLM Service (no keys)
• OpenAI or Azure OpenAI (enter org’s API keys)
• Optionally add Google Gemini via Store spoke. 
servicenow.com
store.servicenow.com
Test prompt returns a valid completion.
4. AI Guardian	Open AI Guardian › Policies; enable the ServiceNow Default policy pack (toxicity, PII redaction). Map any custom sensitive columns for masking.	Red/blocked prompts show correct policy messages.
5. AI Search readiness	Ensure knowledge bases are indexed by AI Search; enable “AI Search recommendations” on portal record producers.	KB query in Conversational Q&A returns cited articles.
6. Role assignment	Grant now_assist_admin to admins; grant now_assist_user (or product-specific roles) to pilot agents.	Users see the Assist panel in workspaces.
7. Skill enablement	In Admin Console › Features, toggle skills per channel (workspace, portal, VA, mobile). Typical first wave:
✔ Record & chat summarisation
✔ Resolution-note draft
✔ Email & chat reply suggestions
✔ Conversational Q&A	Green “Enabled” status for chosen skills.
8. Prompt tuning / cloning	For any skill, click ••• › Clone to adjust system prompt, shorten/extend outputs, or add RAG data sources in Skill Kit. 
servicenow.com
Custom skill ID appears and can be A/B tested.
9. Validation run	Pick one ITSM incident, one CSM case, one HR case. Verify:
• Summarise produces < 6 bullet points.
• Draft resolution note fills the field.
• Tone switch (formal ↔ casual) works.
• Q&A cites KB.	Screenshots logged; pass/fail recorded.
10. Metrics baseline	Capture Admin Console › Usage Analytics before go-live (assists per day, accept-rate).	Baseline CSV exported.
11. Production rollout	Repeat steps 1-8 in prod; enable skills first for a pilot group (UI Policy or User Criteria).	Pilot users live; monitor for 1 week.
12. Hyper-care & expansion	Review usage/feedback weekly; extend roles to all agents; consider industry-specific Assist apps (ITOM, IRM, FSM, etc.).	Enterprise-wide adoption; steady assist-accept rate > 60 %.

Part B · 30-Minute Hands-On Demo Script
Goal: walk stakeholders through every marquee capability using seeded demo data.

Scene 1 · ITSM Agent (“Sofia”) - Incident flow (8 min)
Log into Service Operations Workspace as itil user Sofia.

Open incident INC0010003 (long activity history).

Click Now Assist panel › Summarise incident → observe concise 5-bullet synopsis.

Ask “Why is this incident high priority?” in Conversational Q&A → note cited change record link.

In the activity stream choose ••• › Generate reply → pick Empathetic tone; send email draft.

Click Resolve → use Draft resolution note; adjust Length › Shorten.

Save & show that Work Notes are auto-filled.

Scene 2 · CSM Agent (“Marie”) - Customer chat + scheduling (6 min)
Switch to Customer Service Workspace; open live chat with customer.

Mid-conversation press Summarise chat; AI returns recap.

Type “Can we reschedule my technician visit to next Tuesday afternoon?” → Virtual Agent engages LLM Scheduling Assistant; date picker appears; confirm new slot.

Use Generate reply with Formal tone, confirm appointment change.

Scene 3 · HR Case Worker (“Liam”) - HRSD Journey accelerator (5 min)
In Employee Center Pro, create HR case “New hire onboarding – Sales Rep”.

From Now Assist panel › Generate Journey Accelerator → prompt: “30-day onboarding plan covering benefits, security training, sales certification.”

Review generated journey tasks; accept & publish.

Ask Conversational Q&A: “When does security training happen?” → answer references generated journey.

Scene 4 · Developer (“Ava”) - App Engine Studio (6 min)
Open App Engine Studio; click Create App w/ GenAI.

Prompt: “An app to track workplace ergonomic assessments with request form and approval flow.”

Review AI-generated data model & workspace preview; click Create.

Open a server-side script; highlight block → Quick Actions › Explain code; observe inline explanation.

Add comment: “Refactor to use GlideRecordSecure” → AI inserts suggested changes.

Scene 5 · Product Owner (“Noah”) - SPM Epic-to-Story (5 min)
Go to Agile Board (EAP); open Epic EPC-24 “Improve Mobile Checkout”.

Click Now Assist › Generate user stories; review suggested backlog (story count, acceptance criteria).

Accept three stories, edit one title, and press Add to sprint.

Wrap-up (0-2 min)
Show Admin Console › Usage Analytics where demo assists appear.

Emphasise governance levers: AI Guardian policies, per-channel toggles, skill cloning.

Follow-up homework for attendees:

Enable AI Search recommendations on your portal and test deflection metrics.

Try cloning the Change-Risk Explanation skill and add custom risk factors.

These two artefacts — the deployment checklist and the guided demo — equip teams to activate every Now Assist capability in Yokohama and showcase tangible outcomes to sponsors and end-users.
