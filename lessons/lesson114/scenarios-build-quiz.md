# Cedar AB-400 scenario review: Build Microsoft Power Platform solutions (version 1.0)

27 items, about 54 minutes. Answer closed-book; record confidence as sure, unsure or guess.

### BS-01 (choose 3)

Sami Nasser's requirement: when a subcontractor marks a job Completed in FieldLink, the matching Cedar work order must show Completed within 15 minutes; Omar Saleh must see the completion on the case timeline; Lina Farah wants the weekly share of jobs completed on time on the existing service dashboard. FieldLink can call an HTTPS endpoint with a signed JSON message. Which THREE components should Karim's architecture include? Each correct answer is part of the solution.

- A. An HTTP-triggered Azure Function that verifies the signature and upserts the work order by its FieldLink ID alternate key.
- B. A PCF control on the Work order form that polls the FieldLink API every minute.
- C. A view and chart of completed work orders added to the existing service dashboard.
- D. A scheduled cloud flow that copies all FieldLink jobs into a new elastic table every 15 minutes.
- E. A server-side step (an asynchronous plug-in or a Dataverse-triggered cloud flow) that adds a completion note regarding the case when a work order becomes Completed.

### BS-02

Lina Farah requires that urgent cases get a first response within 4 business hours. Agents must see a countdown on the case; at 3 hours a warning goes to the queue owner; at 4 hours the case is escalated to Lina. Cedar-Prod has Dynamics 365 Customer Service. What should Karim recommend first?

- A. A service-level agreement with an SLA KPI for first response, a business-hours calendar, warning and failure actions, and the SLA timer on the case form.
- B. A scheduled cloud flow that runs every 15 minutes, calculates elapsed business time and sends the warning and escalation emails.
- C. A PCF countdown control on the case form and a plug-in that escalates when the case is updated.
- D. A rollup column of elapsed minutes and a business rule that escalates when it exceeds 240.

### BS-03 (choose 2)

A work order must not be saved with Status = Completed unless Completion summary contains data. Work orders are updated from the Cedar Operations app, by the FieldLink integration through the Web API and by a cloud flow. The rule must hold in every case. Which TWO approaches each meet the requirement on their own?

- A. A business rule with scope Entity that shows an error on Completion summary when Status is Completed and the summary is empty.
- B. A synchronous plug-in step on Create and Update (PreValidation or PreOperation) that throws InvalidPluginExecutionException.
- C. A client script on the form's OnSave event that calls preventDefault.
- D. An asynchronous plug-in step that sets the status back to In progress.
- E. A cloud flow triggered when a work order is modified that rejects the change.

### BS-04

Cedar will connect chiller sensors at Harbor View. Each sensor sends a reading every 30 seconds, about 20 million rows per day across all sites. Readings are kept for 90 days and must then disappear automatically. Reports read the readings of one site at a time; nobody edits them; no business rules or charts are needed on the readings. Where should the readings be stored?

- A. A standard table with a nightly bulk-delete job.
- B. An elastic table with the site code as partition ID and a time to live of 90 days.
- C. A virtual table over the sensor vendor's API.
- D. No table: the reports call a custom connector to the sensors.

### BS-05

Sami built a solution-aware cloud flow in Cedar-Prod that reads new urgent cases from Dataverse and posts them to a subcontractor channel through the Slack connector. Dana then publishes a tenant data policy for the CRM environments that classifies Dataverse as Business and Slack as Non-business. The next morning Sami's flow is turned off. Sami has the System Customizer role and owns the flow. What explains this, and what is the right next step?

- A. Sami lacks privileges; assign Sami the System Administrator role.
- B. The flow combines connectors from the Business and Non-business groups, so the policy suspended it; redesign the flow within one group or ask Dana to review Slack's classification.
- C. Managed Environments block third-party connectors; remove the managed status from Cedar-Prod.
- D. Dana should create an environment-level data policy for Cedar-Prod that allows Slack.

### BS-06

Omar Saleh is in the Cedar Service business unit and has the role Service Agent – Cedar with Business Unit read access on Case. For one week Omar must help with a single escalated case owned by a user in the Cedar Sales business unit. Nothing else in Cedar Sales may become visible to Omar. What is the least-privilege approach?

- A. Change Case read access in the role to Organization.
- B. Move Omar to the Cedar Sales business unit for the week.
- C. Share that case with Omar (Read, Append, Append To) and remove the share after the week.
- D. Add Omar to an owner team in Cedar Sales that has the Service Agent – Cedar role.

### BS-07

Customers report faults by email in free text. Lina wants every new case to get a suggested Case category (Critical fault, Fault, Complaint, Request, Question). Priority must be High exactly when the category is Critical fault, and auditors must be able to reproduce every priority decision. Which design fits best?

- A. An AI prompt that sets both Case category and Priority directly on the case.
- B. An AI prompt that writes a suggested category and a confidence value; an agent confirms or corrects it; a business rule with scope Entity sets Priority from the confirmed category.
- C. A plug-in that assigns the category from a keyword list; the service agent chooses the priority.
- D. A Copilot Studio agent with generative orchestration that decides category and priority and explains its reasoning in a note.

### BS-08

Sami wants the FieldLink status column to be shown as a coloured stepper (Sent, Accepted, In progress, Completed) on three Work order main forms. Makers must be able to add it to further forms like any other control, and the stored value must stay the standard choice column. Which experience should Karim choose?

- A. A code component (PCF) bound to the FieldLink status column.
- B. A generative page embedded in each form.
- C. An HTML web resource on each form that reads the column through parent.Xrm.
- D. A code app opened from a command button.

### BS-09

Lina's managers work in Microsoft 365 Copilot all day. They want to ask "Which urgent cases at Harbor View are close to breaching?" and get a compact card per case with an Open in Cedar Operations button, without leaving the chat. Cedar already runs its own remote MCP server for agents. What should Karim add?

- A. A generative page in the Cedar Operations app.
- B. An MCP app widget: a UI resource returned with the MCP server's case tool and rendered by Microsoft 365 Copilot.
- C. A Power Pages site with a case list.
- D. A canvas app pinned in Teams.

### BS-10

Two agents are planned. The dispatch advisor ranks subcontractors with Cedar's own Python scoring code, must be evaluated in CI with an SDK against a fixed data set, traced end to end in Azure and called from Cedar's API; Karim's developers build it. The service-policy assistant answers employees' questions in Teams from SharePoint documents; Dana's makers build and maintain it. Which platforms fit?

- A. Both in Microsoft Copilot Studio.
- B. Dispatch advisor in Microsoft Foundry; service-policy assistant in Microsoft Copilot Studio.
- C. Dispatch advisor in Microsoft Copilot Studio; service-policy assistant in Microsoft Foundry.
- D. Both in Microsoft Foundry.

### BS-11

The Case main form, Lina's Case – Escalation form and the Case – Mobile form must all show a form notification when Repeat fault is Yes and clear it when the value changes to No. The fields keep their standard look. Karim wants one implementation, versioned in the solution. What should he design?

- A. One namespaced JavaScript web resource with OnLoad and OnChange handlers, registered on each of the three forms.
- B. A PCF control bound to Repeat fault on each form.
- C. A generative page for each form.
- D. A canvas component library with a notification component.

### BS-12 (choose 2)

The FieldLink job lookup must be usable from a solution cloud flow, a canvas app, Cedar's Foundry dispatch advisor and developers' MCP-capable tools in VS Code. The Copilot Studio service assistant must use it too, under Cedar's data policies and solution ALM. One Azure-hosted implementation will serve all of them. Which TWO components should Karim design?

- A. A custom connector built from the FieldLink OpenAPI definition, used by the flow and the canvas app.
- B. A remote MCP server that exposes the job lookup as a tool, which Copilot Studio reaches through an MCP connector.
- C. A virtual table over FieldLink jobs.
- D. A Dataverse custom API that calls FieldLink synchronously from its plug-in.

### BS-13

Coordinators (from a command button), a cloud flow and the FieldLink integration must all perform "Close work order with completion summary". Inputs: the work order and a summary text. Result: closed or already closed. The rules must run on the server in one transaction. How should Karim design it?

- A. A custom API that is an action (Is Function = No), bound to cedar_workorder, with a Summary string parameter and a Result string response, implemented by a plug-in in the main operation.
- B. A custom API that is a function (Is Function = Yes) with the same parameters.
- C. A PreOperation plug-in step on Update of work order that reads the summary from a column.
- D. An instant cloud flow that all three clients call.

### BS-14

Cedar's Copilot Studio service assistant (standard harness) needs a tool that returns a work order's status, scheduled time and subcontractor. The steps are fixed: read the row, map the values, answer. The tool must answer the agent in real time. What should Karim build?

- A. An agent flow with the trigger When an agent calls the flow and the action Respond to the agent, published and added to the agent as a tool.
- B. A scheduled Power Automate cloud flow that writes all statuses to a table every 5 minutes for the agent to search.
- C. A workflow on the GitHub Copilot harness, added as a tool.
- D. An AI prompt that reads the work order and phrases the answer.

### BS-15

When a work order is dispatched, Dataverse must notify FieldLink's sender service and Cedar's finance service. FieldLink's side is sometimes offline for hours; no event may be lost; each service processes at its own pace. Which design should Karim recommend?

- A. A webhook to FieldLink's sender endpoint, registered on an asynchronous step.
- B. An Azure Service Bus topic registered as a service endpoint on an asynchronous step, with one subscription per consuming service.
- C. An event hub that both services read as a stream.
- D. A synchronous plug-in that calls both services over HTTPS.

### BS-16 (choose 2)

Sami's team will build the Dispatch Planner as a Power Apps code app in React and TypeScript that uses Dataverse and the FieldLink custom connector. Which TWO statements must Karim include in the design?

- A. An admin must turn on Enable code apps in each environment that will host the app.
- B. Users who run the app need a Power Apps Premium licence (or pay-as-you-go must be enabled).
- C. The app's source is kept in Git through Power Platform Git integration.
- D. The FieldLink API key can be kept in the app's configuration file because code apps run inside Dataverse.

### BS-17

Cedar's Foundry dispatch advisor answers coordinators' questions about open work orders in Dataverse. Dana's rule: a coordinator may only get answers about rows the coordinator's own security role allows reading, and the advisor must never change data. Which identity design meets the rule?

- A. An application user with the System Administrator role, plus a system prompt that tells the model which rows to hide.
- B. Delegated access: the advisor reaches Dataverse on behalf of the signed-in coordinator (for example through the Dataverse MCP server or the Ask API), with only read tools allowed.
- C. A shared service principal with Organization-level read on Work order.
- D. The coordinator's user name and password stored in the agent's settings.

### BS-18

FieldLink events travel through a Service Bus topic. Lina needs the on-call developer to be notified within 15 minutes whenever any message is dead-lettered. What should Karim configure?

- A. A Power Platform Monitor alert rule on the Cedar Operations app.
- B. An Azure Monitor metric alert on the topic's dead-lettered message count, with an action group that notifies on-call.
- C. Data export to Application Insights and a daily log query.
- D. The weekly usage insights digest of the Managed Environment.

### BS-19

Karim imports the managed solution CedarServiceCore 1.12 into a new environment, Cedar-UAT. The import fails with missing dependencies, including the custom connector for FieldLink and two environment variables of the connector. What should he do?

- A. Import the managed CedarConnectors solution first, then import CedarServiceCore again.
- B. Import CedarServiceCore as an unmanaged solution instead.
- C. Remove the flows that use the connector from CedarServiceCore and retry.
- D. Create the connector by hand in Cedar-UAT, then import CedarServiceCore.

### BS-20

Release 1.12 changed the Work order – Dispatch form in Cedar-Dev and was imported into Cedar-Test with Upgrade. Testers still see the old layout. Last week Dana fixed a label directly on that form in Cedar-Test. What is the cause, and what should Karim do?

- A. The release was imported with Update; import it again with Upgrade.
- B. Dana's edit created an unmanaged active layer above the managed layer; check the form's solution layers, remove the active customization and make the label fix in Cedar-Dev.
- C. The version number wasn't increased; export again with a higher version.
- D. Publish all customizations in Cedar-Test.

### BS-21 (choose 2)

The FieldLink base URL differs in Cedar-Dev, Cedar-Test and Cedar-Prod. The FieldLink client secret must never be stored in Dataverse or in the solution. The release pipeline must set the values without anyone editing flows. Which TWO should Karim use?

- A. A Text environment variable for the base URL, with values supplied per environment by a deployment settings file and no current value left in the exported solution.
- B. A Secret environment variable that references the client secret in Azure Key Vault.
- C. A Text environment variable whose default value holds the client secret.
- D. A connection reference for the base URL.

### BS-22

Cedar's Production stage in Power Platform Pipelines must meet three rules: makers request deployments but have no rights in Cedar-Prod; Lina approves each deployment; imports run only Tuesday to Thursday between 06:00 and 15:00 UTC. Which configuration meets all three?

- A. Is Delegated Deployment with a service principal and an approval flow on OnApprovalStarted, plus Pre-deployment Step Required with a flow on OnPreDeploymentStarted that checks the window.
- B. Pre-export Step Required on the Production stage only.
- C. A flow on OnDeploymentStarted that emails Lina.
- D. System Administrator for makers in Cedar-Prod and a written instruction to deploy only in the window.

### BS-23

Karim's Azure DevOps job must build a managed solution from source control, fail on High solution checker issues, and deploy to Cedar-Test so that components removed from source are deleted there. Which task order is correct?

- A. Tool Installer → Pack Solution → Checker → Import Solution (holding solution) → Apply Solution Upgrade.
- B. Pack Solution → Tool Installer → Import Solution → Checker.
- C. Tool Installer → Export Solution → Import Solution (no holding solution).
- D. Tool Installer → Checker → Pack Solution → Import Solution (holding solution) → Apply Solution Upgrade.

## CASE-1: Cedar release 3: customer self-service and finance view

Cedar Service plans release 3. Facilities teams at Harbor View Medical Center and Olive Grove Schools (external customers without Cedar work accounts) want to report faults and follow the status of their own sites. Finance wants each account in Cedar Operations to show its invoice history from CedarLedger, an Azure SQL database that remains the system of record; finance does not allow copies of invoice data. Omar Saleh's service desk wants an internal agent in Microsoft Teams that answers questions from Cedar's service policies in SharePoint; Dana Khoury's makers will build and maintain it. Dana also wants a daily email to Dana and two colleagues when the availability of the Cedar Operations app drops below 95%. Cedar-Prod is a Managed Environment. Lina Farah's constraint for the release: use configuration and low-code first; custom code only where a requirement cannot be met otherwise.

### CS-1

Case study CASE-1. Which component should Karim propose for the facilities teams of Harbor View and Olive Grove?

- A. A Power Pages site with table permissions that limit each contact to the cases and sites of their own account.
- B. Share the Cedar Operations model-driven app with the customers' email addresses.
- C. A code app that the customers open with their own Microsoft 365 accounts.
- D. A Copilot Studio agent in Cedar's Teams tenant.

### CS-2

Case study CASE-1. How should the CedarLedger invoice history appear on the account in Cedar Operations?

- A. A nightly import into a standard Invoice history table.
- B. A virtual table over the CedarLedger Azure SQL table, created with the virtual connector provider, shown in a subgrid on the account form.
- C. An elastic table filled by a cloud flow every hour.
- D. A custom connector and a canvas app embedded on the account form.

### CS-3

Case study CASE-1. Which platform fits the service desk's policy agent?

- A. Microsoft Copilot Studio, with the SharePoint policies as knowledge and the Teams channel.
- B. Microsoft Foundry, with a Python project that indexes SharePoint.
- C. A code app with a chat page.
- D. A generative page with a search box.

### CS-4

Case study CASE-1. How should Dana be told about Cedar Operations availability?

- A. A Power Platform Monitor alert rule on the app's availability metric with Is under 95 and email to Dana and two colleagues.
- B. A synthetic test in Azure every minute with an SMS action group.
- C. A plug-in that emails Dana when a form fails to load.
- D. A Dataverse audit log review every morning.

