High-Level Test Cases for Now Assist in Yokohama-Washington DC Release

Now Assist (Yokohama Release) – High-Level Test Cases
Below is a set of high-level test cases derived from the Yokohama-Washington DC release notes. These focus on the Now Assist for IT Service Management (ITSM) skills currently in use (Incident Summarization and Knowledge Management) and anticipated Now Assist Creator features. Each test case is designed to prove basic functionality of the new or enhanced capabilities in this release.
Now Assist ITSM – Incident Summarization Skill
Incident Summary Generation (Basic) – Verify that the Incident Summarization skill produces a coherent summary of an incident’s details. Create a test incident with a detailed description and multiple updates, then invoke Now Assist’s incident summarization (e.g. via the Agent Workspace panel). Expected: The AI-generated summary accurately reflects the key information and latest status from the incident’s text content
inry.com
. This ensures the summarization feature is functioning for general use.
Summarization with Image Attachment – Verify that Now Assist can analyze image attachments in an incident and include their information in the summary. Attach a screenshot (PNG/JPEG) containing relevant text or error messages to an incident and trigger the summarization skill. Expected: The summary incorporates data extracted from the image (using ServiceNow’s Document Intelligence capability to read text in PNG/JPEG attachments)
inry.com
. For example, if the screenshot contains an error code, the summary should mention that code. This confirms the new attachment analysis feature in Yokohama.
Voice Input and Follow-Up Queries – Verify that agents can use voice-to-text and multi-turn conversations with Now Assist. Enable the Voice Input setting for Now Assist and use a microphone to ask for an incident summary verbally
plat4mation.com
. After receiving the summary, ask a follow-up question (textually or via voice) such as “Give more details about the resolution.” Expected: Now Assist successfully transcribes the voice input to text and provides the incident summary. On the follow-up, Now Assist remembers the context (multi-turn Q&A) and elaborates on the previous summary
plat4mation.com
. This ensures the hands-free input option and conversational context feature work as intended.
Now Assist ITSM – Knowledge Management Skills
Knowledge Article Suggestions – Verify that Now Assist suggests relevant knowledge base articles for a given incident. With the Now Assist Knowledge skill enabled, open or create an incident describing a common issue that has a known KB article. Expected: The assistant recommends one or more relevant knowledge articles in context (contextual help), allowing the agent to view or attach them
inry.com
. This confirms that AI-driven contextual recommendations are working to deflect or resolve issues using existing knowledge.
Generate Knowledge from Single Incident – Verify AI-generated knowledge article drafting from an incident. Resolve an incident that lacks a linked knowledge article, then use the “Generate Knowledge Article” option from the Now Assist panel or incident actions. Expected: Now Assist creates a draft knowledge article summarizing the incident and its resolution steps
thecloudpeople.com
. The draft should capture the problem and solution in a concise format (for example, a summary of the incident’s cause and fix). This validates the knowledge article generation feature introduced in Yokohama.
Generate Knowledge from Multiple Cases – Verify that Now Assist can consolidate multiple records into one knowledge article. Identify two or more incidents (or cases) about the same recurring issue. Use the Yokohama feature to generate a single knowledge article from these multiple cases
oit.princeton.edu
. Expected: The resulting AI-generated article includes the common problem summary and aggregated resolution details drawn from all selected incidents. This tests the new capability of multi-case knowledge article generation in the release.
Multi-Language Knowledge Article Generation – Verify knowledge article generation in languages other than English. Ensure the instance has multi-language support (e.g. dynamic translation or a non-English knowledge base). Create an incident in another language (for example, Spanish) or switch the target knowledge base’s language. Invoke Now Assist to generate a knowledge article from that incident. Expected: The content is generated in the appropriate language or is accurately translated via AI, aligned with the multi-language support mentioned in the release
oit.princeton.edu
. For instance, an incident described in Spanish should yield a Spanish draft article. This confirms the AI respects language settings when creating knowledge content.
AI-Assisted Knowledge Article Editing – Verify the Now Assist context-menu for editing knowledge content. Open an existing knowledge article and highlight a portion of text (for example, a long paragraph or a complex explanation). Use the Now Assist context menu option “Summarize” or “Improve Writing” (as available in Yokohama) on the selected text
plat4mation.com
oit.princeton.edu
. Expected: The assistant provides a suggested simplified summary or a rewritten version of the highlighted content. The suggestion should maintain the original meaning while improving clarity or conciseness. This test ensures the new inline AI editing tool for knowledge articles works correctly.
Recommended Reply Suggestion – Verify AI-recommended responses for user communications (if applicable to ITSM). On an incident with an incoming customer comment or inquiry, use Now Assist to get a recommended reply draft. (For example, the user asks for an update; the agent triggers Now Assist in the comments field.) Expected: Now Assist generates a context-aware reply, possibly allowing tone selection (e.g. formal or friendly)
plat4mation.com
thecloudpeople.com
. The draft reply should address the user’s question using incident details. This confirms the system’s ability to suggest email/chat responses to improve agent efficiency.
Now Assist Creator – Generative AI for Developers
Role Permission Check (Now Assist Creator) – Ensure only authorized users can access Now Assist Creator features. Attempt to use the Now Assist development tools with and without the required role. Steps: Log in as a developer without the now.assist.creator role and verify that AI-assisted development options (like “Open with Now Assist” in script fields or flow designer) are not available. Then grant the now.assist.creator role and repeat. Expected: Only users with the proper role see and can invoke the generative AI features
linkedin.com
. This test validates the role-based access control for Now Assist Creator per release notes.
Text-to-Code Generation – Verify that Now Assist can generate code from a natural language prompt. In a development environment (e.g. Script Include form or IDE module), use the “Open Code with Now Assist” function. Provide a plain English prompt describing a desired script (for example: “Write a Business Rule script to auto-close incidents in Resolved state after 7 days”). Expected: The AI returns a snippet of script code that fulfills the described requirement (e.g. a JavaScript function using appropriate ServiceNow APIs)
linkedin.com
. The code should be syntactically correct and contextually relevant, demonstrating the Text-to-Code capability.
Code Autocompletion Suggestion – Verify AI-assisted code completion in-line. In a script editor (such as writing a Script Include or Flow Action script), begin typing a function or API call and pause (e.g. type the start of a GlideAggregate query). Expected: Now Assist suggests how to complete the code or the next lines (for instance, it may suggest the query conditions or .query() call to finish the snippet)
linkedin.com
. Accept the suggestion and ensure it inserts correctly. This confirms the code completion feature is working to speed up development.
Code Refactoring Assistance – Verify that Now Assist can refactor or improve existing code. Take an existing script (for example, a moderately complex business rule or script include). Select a portion of the code (or the whole function) and invoke the Now Assist prompt for optimization or best practices (e.g. “Optimize this code for efficiency”). Expected: The AI returns a refactored version or suggestions (such as using better variable names, more efficient queries, or improved error handling)
linkedin.com
. The developer can then review and decide to apply these changes. This tests the AI’s ability to review and enhance code per the new release capabilities.
Flow Creation from Description – Verify automated flow generation from a natural language description. In Flow Designer or the new Workflow Studio, use Now Assist to create a flow. Provide a high-level description of a simple workflow (for example: “When a new incident is created with priority 1, notify the on-call team via email and Slack”). Expected: Now Assist constructs a draft flow with appropriate triggers and actions reflecting the description (e.g. an Incident table trigger for new records, and email + Slack notification steps). The flow should be created or outlined for the developer to review and activate. This validates the Flow Generation/Intelligence aspect of Now Assist Creator introduced in Yokohama, allowing creators to build automation with minimal manual configuration
linkedin.com
linkedin.com
.
Each of these high-level test cases targets a key enhancement from the Yokohama release notes. Executing them will confirm that the Now Assist ITSM features (incident summarization and knowledge management improvements) work as expected in your environment, and that the upcoming Now Assist Creator capabilities (for code and flow generation) are ready to be used for general ServiceNow development. All tests assume a baseline out-of-the-box configuration (no special integrations or customizations), aligning with the provided scenario.
