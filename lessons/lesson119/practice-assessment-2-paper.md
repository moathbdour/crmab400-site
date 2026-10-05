# Cedar AB-400 practice assessment 2 (version 1.0)

48 items in 3 sections, 100 minutes. Write the elapsed time at every checkpoint. No points are lost for a wrong answer: never leave an item blank.

## Section 1: 40 questions

You can flag items and review them before you leave this section; after you leave it you can't return. Budget 74 minutes, 6 of them for review.

### 1. PB-01

A plug-in step on Update of Work order runs as the application user Cedar Automation (Run in User's Context). The plug-in writes cedar_changedby, which must name the person who actually started the change. Which value should it use?

- A. context.InitiatingUserId
- B. context.UserId
- C. context.BusinessUnitId
- D. The ownerid from the work order's pre-image.

### 2. PB-02

On the Case form, when Priority changes to High, a handler must make Escalation reason required. Dana's makers must be able to see the handler in the form designer and switch it off without a code change. How should Karim register it?

- A. In OnLoad, call formContext.getAttribute("prioritycode").addOnChange(cedar.caseForm.onPriorityChange).
- B. In the form designer: Events → Priority → On Change, library cedar_/case.js, function cedar.caseForm.onPriorityChange, Enabled, Pass execution context as first parameter.
- C. Register it in the designer and also with addOnChange in OnLoad, so it still runs if one registration is removed.
- D. In OnLoad, call setValue on Priority so that OnChange runs and registers the handler.

### 3. PB-03

Lina wants every Harbor View Medical Center case to show a countdown to its 4-hour first-response target, warn the agent after 3 hours, mark the case as breached at 4 hours, and stop the clock while the case status reason is Waiting for Details. Customer Service is installed in Cedar-Prod. What should Karim do?

- A. Write a plug-in on case Create that calculates the deadline and an Azure Function on a timer that marks breached cases every five minutes.
- B. Add a cloud flow per case with a Delay until action that sets a Breached column after four hours.
- C. Configure a service level agreement with an SLA item for first response (warning after 3 hours, failure after 4), pause and resume on the Waiting for Details status reason, and the SLA timer on the Case form.
- D. Build a PCF countdown control that calculates the remaining time from createdon on the form.

### 4. PB-04

A case must not be resolved while any of its work orders is still open. Agents resolve cases from the form's Resolve Case dialog, and a cloud flow resolves stale cases. Where should Karim enforce the rule?

- A. A plug-in step on Update of incident with filtering attribute statecode.
- B. A client script on the Resolve Case dialog.
- C. A synchronous plug-in step on the Close message of incident that queries open work orders and throws InvalidPluginExecutionException with a clear message.
- D. A plug-in that runs the check and, if the query fails, logs the error and lets the resolution continue.

### 5. PB-05

Every approved subcontractor invoice must be published to the Azure Service Bus queue sbq-cedar-invoices with the execution context, and the finance consumer may be offline for hours. Karim needs custom logic to decide which invoices qualify, so he writes a plug-in. Which implementation is right?

- A. Use the Service Bus SDK in the plug-in with a connection string in the step's unsecure configuration.
- B. Call the finance consumer's HTTPS endpoint synchronously in PreOperation so approval fails when finance is offline.
- C. Post to the queue and, in the same plug-in, update the invoice status and send an email.
- D. Get IServiceEndpointNotificationService from the service provider and call Execute(new EntityReference("serviceendpoint", endpointId), context) in an asynchronous step that does nothing else.

### 6. PB-06

Karim designs a reusable five-star rating control for the Satisfaction column (cedar_satisfaction, whole number) on several forms, shipped in CedarServiceCore. In index.ts, how should the control give a new value to the form?

- A. Call context.webAPI.updateRecord on the current case with the new value.
- B. Set context.parameters.satisfaction.raw = value in updateView.
- C. Edit the generated ManifestTypes.d.ts to add a setValue method.
- D. Keep the value, call notifyOutputChanged(), and return { satisfaction: value } from getOutputs().

### 7. PB-07

A plug-in on Create of Work order must store, on the new row, how many open work orders its site already has across all coordinators, including rows the caller can't see, without revealing anything else about them. How should it query?

- A. factory.CreateOrganizationService(context.UserId) and RetrieveMultiple with ColumnSet(true).
- B. factory.CreateOrganizationService(null) (SYSTEM) with an aggregate FetchXML count filtered by site and status, storing only the number.
- C. A ServiceClient created in the plug-in with a client secret from unsecure configuration.
- D. HttpClient to the Web API with the caller's token.

### 8. PB-08 (choose 2)

Cedar-Prod is a Managed Environment. The tenant data policy puts Dataverse and Office 365 Outlook in Business and the FieldLink Jobs custom connector in Non-business. An environment data policy for Cedar-Prod blocks HTTP. Sami's new flow in Cedar-Prod reads work orders with Dataverse and creates jobs with FieldLink Jobs. Which two statements are true?

- A. The flow can't be saved or turned on (or is suspended) because it combines a Business and a Non-business connector under the tenant policy.
- B. The environment policy can move FieldLink Jobs into Business for Cedar-Prod and so override the tenant policy.
- C. Assigning Sami the System Administrator role in Cedar-Prod lets the flow run.
- D. To allow the flow, an admin must put FieldLink Jobs in the same group as Dataverse in every policy that applies to Cedar-Prod.
- E. Sharing the flow with a team in another business unit avoids the policy.

### 9. PB-09

The rating control works in Cedar-Dev after pac pcf push. It must now reach Cedar-Test with the Case form that uses it, and later versions must replace it cleanly. What should Karim do?

- A. Run pac pcf push against Cedar-Test.
- B. Copy bundle.js into a web resource in Cedar-Test.
- C. Create a solution project (pac solution init, pac solution add-reference to the .pcfproj), build it in Release, raise the control version for each change, and ship the managed solution with the form through the pipeline.
- D. Export the PowerAppsTools_cedar solution that pac pcf push created in Cedar-Dev and import it into Cedar-Test.

### 10. PB-10 (choose 2)

A synchronous PostOperation plug-in on Update of Account takes 1.8 seconds and runs on every account change, but it only needs to recalculate the credit summary when cedar_creditlimit or cedar_paymentterms change. It retrieves the whole account to read those two values. Which two changes should Karim make?

- A. Set the step's filtering attributes to cedar_creditlimit and cedar_paymentterms.
- B. Return early when context.Depth > 1.
- C. Register a post-image with only the two columns and read them from the image instead of retrieving the account.
- D. Add accountid to the filtering attributes so the step always has the key.
- E. Raise the plug-in time-out in the environment settings.

> Checkpoint after PB-10: planned 17 minutes elapsed. Actual: ____

### 11. PB-11

Sami's integration partner needs a message on the topic sbt-cedar-workorders whenever a work order's status or scheduled date changes, including the previous status. No custom logic is needed. What should Karim do?

- A. Write a plug-in that checks whether the status or date changed and posts to the topic.
- B. In the Plug-in Registration Tool, register the Service Bus endpoint, then a step on it: Update of cedar_workorder, filtering attributes statuscode and cedar_scheduledfor, asynchronous, with a pre-image containing statuscode.
- C. Register the step on Update without filtering attributes; the partner ignores other changes.
- D. Register a synchronous step on the Service Bus endpoint so messages are sent before the save completes.

### 12. PB-12

Karim adds the Office 365 Outlook connector to the Dispatch Planner code app so coordinators can email subcontractors from their own mailbox. The app moves Cedar-Dev → Cedar-Test → Cedar-Prod in CedarServiceCore. How should he add the data source?

- A. pa app add data-source with a connection reference (--connection-ref) in the solution (--solution-id), mapped to a connection in each environment through the deployment settings.
- B. pa app add data-source with his own --connection-id from Cedar-Dev, and share that connection with the coordinators.
- C. Put an Outlook app password in the app's configuration and call Microsoft Graph directly.
- D. Add the data source in Cedar-Prod with the CLI after each deployment.

### 13. PB-13

Karim registers the Cedar.Service.Plugins assembly with the Plug-in Registration Tool in Cedar-Dev. The new step needs a non-secret region code and the FieldLink API key. What should he do?

- A. Put both values in the step's unsecure configuration.
- B. Put both in secure configuration and add the step to CedarServiceCore, so the key reaches Cedar-Test with the solution.
- C. Register the step directly in Cedar-Test and Cedar-Prod with the tool, so the key never leaves each environment.
- D. Put the region code in the unsecure configuration, keep the key in Azure Key Vault read through the plug-in's managed identity, and add the step to the unmanaged solution.

### 14. PB-14

Omar's agents receive free-text emails from Harbor View. Lina wants each new case to get a suggested category (Electrical, HVAC, Plumbing, Access, Other) from the email text, and the response deadline must be 4 hours for Priority 1 hospital cases and 8 hours otherwise, auditable for the contract. What should Karim implement?

- A. A prompt in the intake flow that reads the email and sets both the category and the deadline.
- B. A classic workflow with keyword conditions for the category and a prompt for the deadline.
- C. An agent whose instructions say "always use 4 hours for hospitals", which sets both values.
- D. A prompt that suggests the category for the agent to confirm, and a deterministic rule (the SLA or a plug-in) that sets the deadline from priority and customer type.

### 15. PB-15

Karim designs cedar_EstimateTravel, a reusable read-only operation that returns the travel minutes for a work order (bound to cedar_workorder, output TravelMinutes, whole number), called from the code app and flows. How should he build it?

- A. A custom API marked as a function, bound to cedar_workorder, with its plug-in type set on the API and no step registered; the plug-in reads Target from InputParameters and sets OutputParameters["TravelMinutes"] as an int.
- B. A custom API action with a PreOperation step that returns the minutes in an InvalidPluginExecutionException message.
- C. A custom API function whose plug-in also writes the estimate to the work order.
- D. A plug-in on Retrieve of cedar_workorder that adds the minutes to every retrieved row.

### 16. PB-16

In the Dispatch Planner, a coordinator changes only the Scheduled for date of a work order. The grid also shows the localized display name of the Status column in its header. Which implementation is right?

- A. Retrieve the whole row, change the date, send every column back with update, and call getMetadata for each row in the grid.
- B. Call Cedar_workordersService.update(id, { cedar_scheduledfor: value }) with only the changed column, and read the display name once with getMetadata and cache it.
- C. Call update with the whole row object, and read the display name from the option labels returned with each row.
- D. Use fetch with a PATCH request and a bearer token stored in the app's settings.

### 17. PB-17

Two agent requests reach Karim. (1) Lina's service desk wants an employee agent in Teams that answers from the Service handbook SharePoint site, built and maintained by Dana's makers under the tenant's data policies. (2) Karim's team needs an agent that runs their own Python tools, uses a model they evaluate with SDK-based evaluations and tracing in Azure, and ships through their Azure pipeline. Which platforms should they use?

- A. Copilot Studio for (1) and Microsoft Foundry for (2).
- B. Copilot Studio for both.
- C. Microsoft Foundry for both.
- D. Microsoft Foundry for (1) and Copilot Studio for (2).

### 18. PB-18

A .NET job must read 12,000 cases from the last quarter with each case's account name, then call the custom API cedar_ArchiveCases. Which approach is right?

- A. One RetrieveMultiple without paging, reading the account name as a string.
- B. Retrieve each case by ID, and call service.Retrieve("cedar_ArchiveCases", …).
- C. RetrieveMultiple with a QueryExpression and a LinkEntity to account (alias acct), paging with PagingInfo and the paging cookie, reading acct.name from AliasedValue, then Execute(new OrganizationRequest("cedar_ArchiveCases")).
- D. RetrieveMultiple with TopCount = 12000.

### 19. PB-19

The Dispatch Planner must call the bound custom API cedar_RequestDispatch on a work order and show "My work orders" for the signed-in coordinator. Which approach is right?

- A. Call the custom API from a cloud flow with an HTTP request trigger, and filter "My work orders" by the Entra object ID from the token.
- B. Run the logic in a plug-in on Update of the work order's status instead of calling the API.
- C. Add the custom API, and filter "My work orders" by the Entra object ID that the app's context returns.
- D. Find and add both operations with the CLI (pa app find-dataverse-api, then pa app add dataverse-api for cedar_RequestDispatch and WhoAmI), pass the work order ID as the bound row, and filter by WhoAmI's UserId.

### 20. PB-20

Every night FieldLink sends 40,000 job updates for cedar_fieldlinkstatus, a standard table with the alternate key cedar_fieldlinkid. About 1% fail validation in a plug-in. Lina wants the run to finish within 30 minutes, every valid row saved, and a list of the failures. Which approach meets the goal?

- A. UpsertMultipleRequest batches of 1,000 rows sent in parallel, logging the batches that fail.
- B. One ExecuteTransactionRequest with all 40,000 upserts.
- C. UpsertMultiple batches; when a batch fails, retry it three times.
- D. ExecuteMultipleRequest batches of up to 1,000 UpsertRequest messages with ContinueOnError = true, several batches in parallel, collecting each failed request's fault.

> Checkpoint after PB-20: planned 34 minutes elapsed. Actual: ____

### 21. PB-21

The FieldLink Jobs API now accepts only Microsoft Entra ID tokens issued for the calling user, and Lina wants no client secret to rotate for the connector. What should Karim configure in the custom connector?

- A. OAuth 2.0 with the client credentials grant, so no user sign-in is needed.
- B. API key authentication, with the app registration's client secret as the key.
- C. OAuth 2.0 with Microsoft Entra ID, using the connector's managed identity option (a single-tenant app registration with a federated credential) instead of a client secret.
- D. Basic authentication with Dana's account.

### 22. PB-22

Karim added a second input, Urgent (Yes/No), to the solution-aware instant flow "Cedar – Notify subcontractor" (Power Apps trigger). The Dispatch Planner still builds, but its call now fails with a missing-parameter error. What should he do?

- A. Change the trigger to "When an HTTP request is received" and call the flow's URL with fetch.
- B. Turn the flow off and on so the app picks up the new input.
- C. Run pa app add flow --flow-id <id> again to regenerate the typed service and model, then pass Urgent in Run().
- D. Edit the generated service file by hand to add the parameter.

### 23. PB-23 (choose 2)

Karim must load about 2 million sensor readings a day into cedar_sensorreading, an elastic table, and 800 contract price changes into the standard table cedar_contract, where all price changes must succeed or none. Which two designs are right?

- A. Readings: ExecuteTransactionRequest batches of 1,000 rows so that each batch is atomic.
- B. Prices: ExecuteMultiple with ContinueOnError = true and 100 rows per request.
- C. Readings: CreateMultiple with about 100 rows per request, sent in parallel, finding failed rows in the error details.
- D. Prices: UpdateMultiple with the 800 rows in one request; on a standard table any error rolls back the whole request.
- E. Readings: one Create request per row from 50 parallel threads.

### 24. PB-24

Cedar Service Assistant, a Copilot Studio agent on the standard harness, must look up a site's open work orders in Dataverse during a conversation and use the list in its next answer. Dana's makers will maintain the tool. What should Karim build?

- A. An automated Power Automate cloud flow with the Dataverse trigger "When a row is added, modified or deleted".
- B. An agent flow with the trigger "When an agent calls the flow" and a "Respond to the agent" action, kept within the real-time response limit.
- C. A scheduled cloud flow that copies open work orders to a SharePoint list every hour for the agent to read.
- D. A classic Dataverse workflow run on demand.

### 25. PB-25

The Dispatch Planner must let a coordinator ask the published Copilot Studio agent Cedar Service Assistant a question and show the answer in a side panel. The agent's name differs between Cedar-Test and Cedar-Prod. What should Karim use?

- A. The Microsoft Copilot Studio connector's ExecuteCopilotAsyncV2 (Execute Agent and wait) with the message and the agent name read from an environment setting, showing the reply as text.
- B. A Direct Line secret in the app's code to open a Web Chat session.
- C. The connector's operation, inserting the reply with innerHTML so its links work.
- D. An older connector operation with the agent's name hard-coded.

### 26. PB-26

Rafi's Python script calls the unbound custom API cedar_RebuildSiteStats, which runs about 100 seconds. The script must not hold its HTTP connection open, and Rafi wants to know when the operation has finished. Cedar-Test allows preview features. What should the script do?

- A. Ask Karim to register the custom API's plug-in step as asynchronous so that the call returns at once.
- B. Send the request with the header Prefer: respond-async, keep the Location URL from the 202 Accepted response, and poll that status monitor until it reports completion.
- C. Raise the requests time-out to 300 seconds.
- D. Ask Dana to raise the plug-in execution limit to five minutes in Cedar-Test.

### 27. PB-27

The FieldLink sync function in func-cedar-fieldlink logs a custom event SyncFailed, with the subcontractor code, to appi-cedar-prod for every work order it can't sync. Sami must be told within 15 minutes when more than 20 work orders fail in an hour, with the subcontractor codes involved. What should Karim set up?

- A. A Power Platform Monitor alert rule on the sync flows in Cedar-Prod.
- B. The Dataverse export to Application Insights with an alert on failed plug-in executions.
- C. A cloud flow that sends Sami a Teams message for each failed work order.
- D. An Azure Monitor log search alert on appi-cedar-prod: KQL over customEvents named SyncFailed in the last hour, summarised by subcontractor code, threshold more than 20, evaluated every 5 minutes, with an action group that notifies Sami.

### 28. PB-28

Rafi needs a scheduled Python job, with no user present, that creates 5,000 visit rows in Dataverse each night. Which code is right?

- A. InteractiveBrowserCredential and a loop that creates one row per request.
- B. CrmServiceClient with AuthType=Office365 and a stored password.
- C. DataverseClient(url, credential) from PowerPlatform-Dataverse-Client with an azure-identity certificate or client-secret credential for an application user, and one create call with the list of 5,000 records, which uses bulk messages.
- D. The requests library with the application user's password in a header.

### 29. PB-29

In Cedar-Test the Dispatch Planner must read work orders from a different SharePoint site and list than in Cedar-Dev (data-source arguments), and the FieldLink map base URL also differs per environment. Which design is right?

- A. Hard-code both values and build the app once per environment.
- B. @envvar:<schema name> references for the SharePoint dataset and table in power.config.json, the map URL read at runtime from the Dataverse environment variable tables, and the values set per environment in the deployment settings file.
- C. Both values in a .env file bundled with the app.
- D. A connection reference for the map URL and an @envvar reference for the FieldLink API key.

### 30. PB-30

FieldLink publishes an OpenAPI 3.0 definition of 2.4 MB with 160 operations. Karim needs a custom connector with three of them: ListSubcontractors, CreateJob and GetJob. CreateJob's priority parameter takes Low, Normal or High, values FieldLink hasn't changed in years. What should he do?

- A. Convert the definition to OpenAPI 2.0, trim it to the three operations (well below 1 MB), import it, and declare priority with enum: [Low, Normal, High].
- B. Import the 3.0 file as it is from its URL, and add x-ms-dynamic-values on priority.
- C. Convert and trim, then add x-ms-dynamic-values on priority calling a new ListPriorities operation that FieldLink would have to build.
- D. Use Import from GitHub with the URL of FieldLink's own repository.

> Checkpoint after PB-30: planned 51 minutes elapsed. Actual: ____

### 31. PB-31

The flow "Cedar – Dispatch to FieldLink" has a Try scope. A Catch scope must run when any action in Try fails or times out, post the failed action's error to Teams, and the run must still show as Failed. How should Karim configure Catch?

- A. Catch runs after Try "has failed" only and ends with Terminate (Succeeded).
- B. Remove Catch and set a retry policy of four on every action.
- C. Put Terminate (Failed) inside the Apply to each in Try.
- D. Catch runs after Try "has failed" and "has timed out", finds the failed actions with result('Try'), posts to Teams, then ends with Terminate (Failed).

### 32. PB-32

The Dispatch Planner must show FieldLink's live map page, https://maps.fieldlink.example, in an iframe on its Route tab. Content Security Policy enforcement for code apps is on in Cedar-Prod. Which directive must Dana extend?

- A. frame-ancestors, with https://maps.fieldlink.example.
- B. connect-src, with https://maps.fieldlink.example.
- C. frame-src, with https://maps.fieldlink.example.
- D. Turn enforcement off for code apps and keep reporting on.

### 33. PB-33

CedarServiceCore 3.4 (managed) was imported into Cedar-Test, but the Case main form still shows the old tab layout. The form's solution layers show an Active (unmanaged) layer above CedarServiceCore; Dana confirms someone edited the form directly in Cedar-Test last week. What should Karim do so the managed form takes effect?

- A. Publish all customizations in Cedar-Test.
- B. Import CedarServiceCore 3.4 again as an update.
- C. Remove the active (unmanaged) layer of the Case main form from its solution layers, then check the form.
- D. Raise the version to 3.5 and import it as an upgrade.

### 34. PB-34

Three flows in CedarServiceCore need the same "Create FieldLink job" steps, which use the FieldLink Jobs custom connector. Karim moves them into a child flow. What must be true?

- A. The child has an HTTP request trigger, and the parents call its URL with an HTTP action.
- B. The child uses "Manually trigger a flow" and "Respond to a Power App or flow", is in the same solution, is called with Run a Child Flow, and its FieldLink connection is set to the embedded connection under Run only users.
- C. The parents pass their FieldLink connection to the child as an input.
- D. The child stays outside any solution so every maker can edit it.

### 35. PB-35

Karim adds Application Insights telemetry to the Dispatch Planner. Which implementation follows the code apps guidance?

- A. Add the snippet that loads the SDK from the CDN to index.html, and give the connection string through an @envvar reference.
- B. Log every coordinator's name and email in custom events to find slow users.
- C. Create a new Application Insights client in every component's render.
- D. Install the npm package (served from 'self'), initialise it once at start-up, forward sessionLoadSummary and networkRequest from setConfig({ logger: { logMetric } }), add the ingestion origin to connect-src, and log no personal data.

### 36. PB-36

Dana creates a "Connect with service principal" Dataverse connection in Cedar-Test with the app registration Cedar Automation (certificate) for the flow "Cedar – Nightly SLA report". The first run fails because the application isn't a user of the Cedar-Test organization. What should she do?

- A. Create an application user for Cedar Automation's application (client) ID in Cedar-Test and assign it the least-privilege role Cedar Automation.
- B. Add the Dynamics CRM user_impersonation API permission to the app registration and grant admin consent.
- C. Give the app registration the Contributor role on the Azure subscription.
- D. Share her own Dataverse connection with the flow instead.

### 37. PB-37 (choose 2)

In the FieldLink Jobs connector, GetJob returns times as Unix seconds and nests the address three levels deep. Makers want ISO 8601 times and a flat address in every flow, without changes at FieldLink. Which two statements about a custom code (C#) solution are true?

- A. The script can write each response to Application Insights for troubleshooting.
- B. A Script class deriving from ScriptBase overrides ExecuteAsync, calls this.Context.SendAsync to forward the request, and rewrites the response JSON.
- C. The script can reach FieldLink through the on-premises data gateway.
- D. The script must finish within the documented time limit (2 minutes for new connectors when checked) and handle messages up to 1 MB.
- E. A Set property policy template can compute the ISO dates from Unix seconds without code.

### 38. PB-38

Karim's Azure DevOps build pipeline for CedarServiceCore must produce a managed solution from the unpacked files in Git and stop when the solution checker reports critical issues. Which task order is right?

- A. Power Platform Tool Installer → Power Platform Pack Solution (managed) → Power Platform Checker (fail on critical) → Publish Pipeline Artifact.
- B. Power Platform Export Solution from Cedar-Dev → Power Platform Checker → Publish Pipeline Artifact.
- C. Pack Solution → Import Solution into Cedar-Test → Power Platform Checker.
- D. Power Platform Checker → Tool Installer → Pack Solution.

### 39. PB-39 (choose 2)

After a release, two coordinators can't open the Dispatch Planner at all, and others say the Subcontractor list stays empty. Which two steps should Karim take first?

- A. Query Application Insights for the two users' sessions to see why the app didn't load.
- B. Check Monitor in the Power Platform admin center for the app's open failures and their error codes for those users.
- C. Remove and re-share the app with every coordinator.
- D. Turn off CSP enforcement to see whether the list appears.
- E. In browser DevTools, read the Console for CSP violations and the Network tab for the failing request to the connector (apihub) host.

### 40. PB-40

Karim's Foundry agent Cedar Dispatch Advisor must answer coordinators' questions about work orders from Dataverse data through Work IQ, showing only rows each coordinator may see. Which identity design is right?

- A. The agent calls Work IQ with its own app-only token, which sees every work order, and filters the rows in its prompt.
- B. The agent uses a System Administrator application user and instructions that say "only show the user's own work orders".
- C. The agent calls Work IQ on behalf of the signed-in coordinator (a delegated, on-behalf-of token), so Dataverse security decides what is returned.
- D. The agent stores each coordinator's password and signs in as them.

> Checkpoint after PB-40: planned 68 minutes elapsed. Actual: ____

## Section 2: goal set (nightly SLA statistics function)

You answer each item once; you can't return to it.

Problem: Every night at 01:30 UTC an Azure Function in func-cedar-ops-prod must recalculate SLA statistics for about 40,000 cases in Cedar-Prod. Goal: it runs on schedule with nobody present, it authenticates to Dataverse without any stored secret, and a run that started late (for example after a platform restart) is recorded so that Dana can check it.

### 41. PB-41

Solution: a timer trigger with the schedule "0 30 1 * * *"; the function reads the app registration's client secret from an application setting and uses ClientSecretCredential; it logs a warning when the timer's IsPastDue is true. Does the solution meet the goal?

- A. Yes
- B. No

### 42. PB-42

Solution: a timer trigger with the schedule "30 1 * * *"; a system-assigned managed identity registered as an application user in Cedar-Prod; a warning logged when IsPastDue is true. Does the solution meet the goal?

- A. Yes
- B. No

### 43. PB-43

Solution: a timer trigger with the schedule "0 30 1 * * *"; a system-assigned managed identity registered as an application user in Cedar-Prod with a least-privilege role; ManagedIdentityCredential requesting https://<org>.crm.dynamics.com/.default for the Dataverse client; a warning logged when IsPastDue is true. Does the solution meet the goal?

- A. Yes
- B. No

> Checkpoint after PB-43: planned 80 minutes elapsed. Actual: ____

## Section 3: case study

You can flag items and review them before you leave this section; after you leave it you can't return. Budget 18 minutes, 2 of them for review.

### CASE-6: REL-036 subcontractor certificate checks

Release REL-036, subcontractor certificate checks. From 1 January 2027 every subcontractor must hold a valid insurance certificate before Sami Nasser's coordinators may assign it work. On Wednesday 9 December 2026 Sami, Dana Khoury and Karim Aziz agree four requirements. (1) Coordinators need a new code app, Cedar Onboarding Desk, to review uploaded certificates. It must reach Cedar-Test and Cedar-Prod through the existing pipeline with CedarServiceCore and be available to the coordinators, who hold Power Apps Premium licences. (2) Subcontractors upload certificates (PDF) to the Azure Blob Storage container certificates in stcedardocs. An Azure Function, CertificateReader in func-cedar-ops-prod, extracts insurer, policy number and expiry date with a document-analysis service whose key is in kv-cedar-prod. The function is published in Azure API Management (apim-cedar-prod) as the API Cedar Certificates. Makers must be able to call it from cloud flows and the code app, and the function must know which coordinator asked, for its audit log. (3) Karim's Foundry agent Cedar Dispatch Advisor must check a subcontractor's certificate status in the Dataverse table cedar_certificate before it proposes a subcontractor, with read access only and only to tables Dana approves. (4) Before anything is built, Dana wants the design to show which component meets each requirement.

### 44. PB-44

Requirement 4: which component list meets requirements 1–3?

- A. (1) A canvas app; (2) an HTTP action with the API Management subscription key in each flow; (3) the agent calls the Dataverse Web API with a client secret in its code.
- B. (1) A code app in CedarServiceCore; (2) a custom connector created from API Management, with Entra ID authentication, in front of the function; (3) the Dataverse MCP server as an MCP tool of the agent, limited to approved read tools.
- C. (1) A code app; (2) a custom connector; (3) the agent reads a nightly CSV export of cedar_certificate from Blob Storage.
- D. (1) A generative page in the Cedar Service app; (2) a Power Automate HTTP trigger that runs the extraction in the flow; (3) a Copilot Studio agent replaces the Foundry agent.

### 45. PB-45

Requirement 1: how should Karim deploy and share Cedar Onboarding Desk?

- A. Run pa app push against Cedar-Prod and share the app with the coordinators there.
- B. Connect the app to Power Platform Git integration and deploy from the repository.
- C. Build the app and publish the dist folder to a SharePoint site.
- D. pa app push with --solution-id to Cedar-Dev only; Cedar-Test and Cedar-Prod receive the managed CedarServiceCore solution through the pipeline with its deployment settings; share the app with the coordinators.

### 46. PB-46

Requirement 3: how should Cedar Dispatch Advisor reach the certificate data?

- A. Add the Dataverse MCP server of Cedar-Prod as an MCP tool, allow the Foundry client for that environment, limit allowed_tools to the read tools Dana approves, and connect with an identity whose security role reads only cedar_certificate.
- B. Give the agent an application user with System Administrator and instruct it to read only cedar_certificate.
- C. Point the MCP tool at the server's SSE endpoint so answers stream.
- D. Build and host a new MCP server that wraps the Web API with a stored client secret.

### 47. PB-47

Requirement 2: how should Karim give makers a connector for Cedar Certificates?

- A. Export the API's OpenAPI definition from API Management, edit it by hand, and import it as a new connector.
- B. Give makers the subscription key so they can call the API with the HTTP action.
- C. In apim-cedar-prod, open APIs → Cedar Certificates → Power Platform → Create a connector, choose the environment, select OAuth (Microsoft Entra ID), then add the connector to CedarConnectors.
- D. Create a webhook service endpoint for the function in the Plug-in Registration Tool.

### 48. PB-48

Requirement 2: how should the CertificateReader function be secured so that it knows which coordinator asked?

- A. Require a function key and configure it as the connector's API key.
- B. Turn on App Service authentication (Microsoft Entra ID) for the function app, have the user's token forwarded through API Management from the OAuth connector, read the coordinator from X-MS-CLIENT-PRINCIPAL, and read the document-analysis key from kv-cedar-prod with the function's managed identity.
- C. Allow anonymous calls and pass the coordinator's email as a query parameter.
- D. Have the connector send the document-analysis key in a header with each call.

> Checkpoint after PB-48: planned 96 minutes elapsed. Actual: ____

