# Cedar AB-400 practice assessment 1 (version 1.0)

43 items in 3 sections, 100 minutes. Write the elapsed time at every checkpoint. No points are lost for a wrong answer: never leave an item blank.

## Section 1: 35 questions

You can flag items and review them before you leave this section; after you leave it you can't return. Budget 74 minutes, 6 of them for review.

### 1. PA-01

Sami's coordinators must not be able to set a work order to Completed unless at least one note with an image is attached to it. Work orders are updated from the Work order – Dispatch form, by the FieldLink sync through the Web API and by two cloud flows. What should Karim implement?

- A. A client script on the form's OnSave event that cancels the save when no image note exists.
- B. An automated cloud flow on modify that sets the status back to In progress when no image note exists.
- C. A synchronous plug-in step on Update of Work order (filtering column: status), PreValidation stage, that queries the notes and throws InvalidPluginExecutionException when no image is attached.
- D. A business rule with scope Entity that shows an error on the status column when no image note exists.

### 2. PA-02

On the Site – Cedar form, a handler registered on the OnChange event of Site type (Pass execution context as first parameter is ticked) must show the section sec_access on tab tab_general only for hospitals and show the selected Site type label in a form notification. Which code is correct?

- A. const f = Xrm.Page; f.ui.tabs.get("tab_general").sections.get("sec_access").setVisible(isHospital);
- B. const f = executionContext.getFormContext(); f.ui.tabs.get("tab_general").sections.get("sec_access").setVisible(isHospital); f.ui.setFormNotification(f.getAttribute("cedar_sitetype").getText(), "INFO", "sitetype");
- C. const f = executionContext.getFormContext(); f.getAttribute("sec_access").setVisible(isHospital); f.ui.setFormNotification(f.getAttribute("cedar_sitetype").getValue(), "INFO", "sitetype");
- D. document.getElementById("sec_access").style.display = isHospital ? "block" : "none";

### 3. PA-03

A plug-in must stop a Case from being created when the customer account is on credit hold. The check uses only the submitted values and must run before the database transaction starts, so no other synchronous step does work that has to be rolled back. In which stage should Karim register the step?

- A. PostOperation, asynchronous.
- B. PreOperation, synchronous.
- C. MainOperation, synchronous.
- D. PreValidation, synchronous.

### 4. PA-04

Keystone Audit, Cedar's safety auditor, wants a copy of every new Case within minutes. Its API is a simple HTTPS endpoint (about 40 cases a week) that authenticates with a key in an HTTP header and is rarely down for more than a few minutes. Dana wants no new Azure resources and no code outside Dataverse. Which option should Karim recommend?

- A. An asynchronous webhook step on Create of Case, authentication HttpHeader with the key.
- B. A Service Bus queue endpoint and an Azure Function that forwards each message to Keystone.
- C. An Event Hubs endpoint that Keystone reads with its own consumer group.
- D. A synchronous webhook step on Create of Case, authentication HttpHeader with the key.

### 5. PA-05 (choose 2)

Karim's job changes a work order's Scheduled for and links it to the subcontractor account through the Web API. Another process may have changed the row since the job read it; such a change must not be overwritten silently. Which two parts belong in the request?

- A. PATCH [org]/api/data/v9.2/cedar_workorders(<id>) with the header If-Match: *
- B. PATCH [org]/api/data/v9.2/cedar_workorders(<id>) with the header If-Match: W/"<etag read earlier>"
- C. "cedar_subcontractor": "<account id>" in the body
- D. "cedar_Subcontractor@odata.bind": "/accounts(<account id>)" in the body
- E. PUT [org]/api/data/v9.2/cedar_workorders(<id>) with the whole row in the body

### 6. PA-06 (choose 2)

Karim's field control cedar.SignaturePad must store the signature as text in a Multiple lines of text column and use the device camera to photograph a paper signature when the device supports it; on desktops the control must still load. Which two manifest entries should he use?

- A. <property name="signature" of-type="Multiple" usage="bound" required="true" />
- B. <property name="signature" of-type="SingleLine.Text" usage="input" />
- C. <feature-usage><uses-feature name="Device.captureImage" required="false" /></feature-usage>
- D. <external-service-usage enabled="true"><domain>camera</domain></external-service-usage>
- E. <feature-usage><uses-feature name="Device.captureImage" required="true" /></feature-usage>

### 7. PA-07

Finance insists that CedarLedger (Azure SQL) stays the system of record for invoices: no copies in Dataverse. Maya wants the account form to list each customer's invoices in a subgrid, with a view sorted by due date. Which approach should Karim propose?

- A. An elastic table loaded nightly from CedarLedger.
- B. A virtual table over CedarLedger with the SQL Server virtual connector provider, related to account and shown in a subgrid.
- C. A standard table kept current by an hourly cloud flow.
- D. A custom connector called from a canvas app embedded in the account form.

### 8. PA-08

The nightly job cedar-automation authenticates with a certificate and works in Cedar-Dev. In Cedar-Test it gets a token for https://<cedar-test>.crm.dynamics.com/.default without errors, but every Dataverse call fails with "The user is not a member of the organization." What should Karim do?

- A. Add the Dynamics CRM user_impersonation API permission to the app registration and grant admin consent.
- B. Request the token for the Cedar-Dev URL instead.
- C. Create an application user for cedar-automation in Cedar-Test and give it the Cedar Automation security role.
- D. Assign the app registration the Contributor role on the Azure subscription.

### 9. PA-09

The Dispatch Planner code app must start the flow "Cedar – Notify subcontractor" when a coordinator selects Send, passing the work order number. Which flow can the app call?

- A. An automated cloud flow with the Dataverse trigger "When a row is added, modified or deleted".
- B. A scheduled cloud flow that runs every five minutes.
- C. An instant flow with the Power Apps trigger that isn't in a solution, shared with the coordinators.
- D. A solution-aware instant flow with the Power Apps trigger, added to the app with the CLI and regenerated when its inputs change.

### 10. PA-10

A synchronous PostOperation plug-in on Update of Case must write a timeline note when Priority changes from Normal to High, stating the old and the new value. What should Karim register and read?

- A. A pre-image with the alias PreImage containing only prioritycode; compare it with prioritycode in the Target.
- B. A post-image with all columns; compare it with the Target.
- C. No image: retrieve the case inside the plug-in to get the old priority.
- D. No image: read the old priority from the Target.

> Checkpoint after PA-10: planned 19.4 minutes elapsed. Actual: ____

### 11. PA-11 (choose 2)

Two new components: (1) the Azure Function SiteImport runs nightly without a user and writes Site rows; (2) the Dispatch Planner code app must show only the work orders the signed-in coordinator is allowed to see. Dana wants no stored secrets. Which two designs meet the requirements?

- A. SiteImport uses its managed identity, with an application user in each environment holding the least-privilege role Cedar Site Import.
- B. SiteImport uses an app registration with a client secret stored in the function's app settings.
- C. The code app reads work orders through the signed-in user's Dataverse connection, so the user's security roles decide which rows are returned.
- D. The code app reads through one shared service-account connection and filters rows by owner in code.
- E. The SiteImport application user gets System Administrator so that imports never fail on privileges.

### 12. PA-12

FieldLink sends job updates identified by the FieldLink job ID, which is the alternate key cedar_fieldlinkid on Work order. Work orders are always created in Cedar first; an update for an unknown job ID must fail and must not create a row. Which Web API request should the receiver send?

- A. PATCH cedar_workorders(cedar_fieldlinkid='FL-2291') with no conditional header.
- B. POST cedar_workorders with cedar_fieldlinkid in the body.
- C. PATCH cedar_workorders(cedar_fieldlinkid='FL-2291') with the header If-Match: *
- D. PATCH cedar_workorders(cedar_fieldlinkid='FL-2291') with the header If-None-Match: *

### 13. PA-13

On Work order – Dispatch, the Subcontractor lookup must list only accounts whose service line matches the work order's service line, and the filter must follow later changes of the service line. Where and how should Karim register the filter?

- A. In the form OnLoad handler (registered in the designer with Pass execution context), call getControl("cedar_subcontractor").addPreSearch(filterFn) once; filterFn reads the current service line and calls addCustomFilter.
- B. Register filterFn on the lookup's PreSearch event in the form designer's Events tab.
- C. In the service line OnChange handler, call addPreSearch(filterFn) each time the value changes.
- D. In OnLoad, call addOnChange on the service line and then setValue on it, so that the OnChange handler applies the filter.

### 14. PA-14

The custom API cedar_RecalculateContracts takes about 90 seconds. Karim's Python caller times out waiting for the HTTP response. Lina wants the caller freed at once and told the outcome later; a preview feature is acceptable in Cedar-Test. What should Karim do?

- A. Raise the caller's HTTP timeout to five minutes.
- B. Send the request with the header Prefer: respond-async, keep the Location URL from the 202 response and poll it, or subscribe to OnBackgroundOperationComplete.
- C. Register the custom API's plug-in step as asynchronous.
- D. Move the logic to an asynchronous plug-in and raise the sandbox limit to ten minutes in the environment settings.

### 15. PA-15

Karim adds the Dataverse table cedar_workorder to the Dispatch Planner code app as a data source. Which statement about it is correct?

- A. It needs the connection ID of a shared service connection.
- B. It can run FetchXML queries for aggregates.
- C. It runs as the signed-in user, so each coordinator's security roles decide which rows the app returns.
- D. It supports polymorphic lookups such as Customer on Case.

### 16. PA-16

Makers' cloud flows must call cedar_GetSiteRisk with the input SiteCode (text) and receive RiskScore (whole number). The operation doesn't change data. How should Karim define the custom API?

- A. A function (Is Function = Yes), unbound, Is Private = Yes so only flows can see it.
- B. A function bound to the Site table.
- C. A custom process action with the same parameters.
- D. An action (Is Function = No), unbound, request parameter SiteCode of type String, response property RiskScore of type Integer, and an execute privilege to control who can call it.

### 17. PA-17

Sami wants a signature pad on the Work order and Case forms, bound to a text column, reused on both tables and shipped in CedarServiceCore. Which user-experience option should Karim choose?

- A. A code app opened from a button on the form.
- B. A generative page added to the app's navigation.
- C. An HTML web resource placed in an iframe section of each form.
- D. A code component (PCF) field control bound to the column and added to both forms.

### 18. PA-18

FieldLink now requires the header X-FieldLink-Tenant: cedar on every call. Thirty flows use the FieldLink Jobs custom connector, and Lina doesn't want any of them changed. What should Karim add to the connector?

- A. Custom code that adds the header to every request.
- B. A Set HTTP header policy with the value cedar, applied to all operations.
- C. A new connection parameter that each connection owner fills in.
- D. An Azure Function in front of FieldLink that adds the header.

### 19. PA-19

The flow "Cedar – Notify on high priority" uses the Dataverse trigger "When a row is added, modified or deleted" (Modified, Case) and runs about 3,000 times a day. It should run only when a case is saved with Priority High. Which trigger configuration should Karim use?

- A. Select columns: prioritycode; Filter rows: prioritycode eq 1.
- B. No trigger settings; the first action is a Condition on prioritycode.
- C. A trigger condition that compares the previous and the new value of prioritycode.
- D. Filter rows: prioritycode eq 1 only.

### 20. PA-20

Karim's field control cedar.Rating shows stars bound to cedar_satisfaction. He calls notifyOutputChanged at the end of updateView, and the form freezes after the first click. What fixes it?

- A. Render only in init and leave updateView empty.
- B. Call notifyOutputChanged from getOutputs.
- C. Wrap the notifyOutputChanged call in updateView in setTimeout.
- D. Call notifyOutputChanged only from the click handler registered in init, and keep updateView for rendering from the context.

> Checkpoint after PA-20: planned 38.9 minutes elapsed. Actual: ____

### 21. PA-21

The flow "Cedar – Close stale cases" runs on a "Connect with service principal" connection for the Dataverse connector. In Cedar-Test it fails with "Principal user (Id=…, type=8) is missing prvWriteIncident privilege". What should Karim change?

- A. Share Omar's personal Dataverse connection with the flow instead.
- B. Add Write on Case at the needed access level to the application user's role Cedar Automation.
- C. Add the Dynamics CRM user_impersonation API permission to the app registration and grant admin consent.
- D. Assign the System Administrator role to the application user.

### 22. PA-22

When a case is escalated to High, Dana wants a Teams message to the on-call coordinator and a follow-up task. No agent is involved, makers will maintain it, and it must ship in CedarServiceCore. What should Karim build?

- A. An automated Power Automate cloud flow with the Dataverse trigger, in the solution.
- B. A Copilot Studio agent flow with the same steps.
- C. A Copilot Studio workflow on the GitHub Copilot harness.
- D. A classic Dataverse background workflow.

### 23. PA-23 (choose 2)

The Dispatch Planner code app must call the FieldLink status API at https://api.fieldlink.example from the browser and may be embedded only in Cedar's intranet at https://intranet.cedarservice.example. CSP enforcement is on for code apps. Which two settings should Dana add?

- A. https://api.fieldlink.example in connect-src.
- B. https://intranet.cedarservice.example in frame-ancestors.
- C. * in connect-src.
- D. Turn enforcement off and keep reporting on.
- E. https://intranet.cedarservice.example in frame-src.

### 24. PA-24

Lina wants other teams' flows and Azure services to react when Cedar marks a site as unsafe, without Cedar knowing who subscribes. The event carries SiteId and Reason. What should Karim create?

- A. A custom API cedar_OnSiteMarkedUnsafe with a plug-in that updates the site.
- B. A synchronous plug-in that calls each subscriber's webhook.
- C. A custom API cedar_OnSiteMarkedUnsafe with no plug-in type, allowed step type Async Only, request parameters SiteId and Reason, and a catalog assignment in a catalog category.
- D. The same custom API with allowed step type Sync and Async.

### 25. PA-25 (choose 2)

CedarReports, LedgerLine and the FieldLink gateway must each receive work-order completions, and any of them may be offline for hours. Karim registers the Dataverse side with the Plug-in Registration Tool. Which two should he use?

- A. One service endpoint with designation Topic and message format JSON, registered from a SAS connection string with Send rights.
- B. One subscription per consumer on that Service Bus topic.
- C. Three queue endpoints, one per consumer.
- D. A SAS policy with Listen rights for the service endpoint.
- E. A webhook for each consumer.

### 26. PA-26

On the Work orders main grid, a modern command "Send to FieldLink" must be visible only when exactly one row is selected, and must call cedar.workOrder.send with that row's ID. What should Karim configure?

- A. Visible formula that tests Self.Selected.Item only; action Run JavaScript with no parameters.
- B. Visible formula CountRows(Self.Selected.AllItems) = 1; action Run JavaScript with library cedar_/js/workorder.js, function cedar.workOrder.send and parameter SelectedControlSelectedItemIds.
- C. The same visibility, and no execute privilege on the API because hidden buttons can't be clicked.
- D. Edit the classic Send command in the command designer.

### 27. PA-27

An HTTP-triggered Azure Function rebuilds a contract's 2,000 maintenance visits, which takes about six minutes. A cloud flow calls it and fails after about four minutes. What should Karim change?

- A. Raise functionTimeout in host.json to ten minutes.
- B. Return 200 immediately and continue the work in the same invocation.
- C. Use a Durable Functions orchestrator that calls the Dataverse Web API directly.
- D. An HTTP starter that returns 202 with a status URL; a Durable Functions orchestrator runs the rebuild in activity chunks; the flow follows the asynchronous pattern.

### 28. PA-28

Importing CedarServiceCore 1.11 (managed) into Cedar-Test fails with missing dependencies: the custom connector FieldLink Jobs and the environment variable cedar_FieldLinkBaseUrl. What should Karim do?

- A. Import CedarServiceCore as unmanaged.
- B. Remove the connector from CedarServiceCore and import again.
- C. Import the matching managed version of CedarConnectors first, then CedarServiceCore.
- D. Create the connector and the variable by hand in Cedar-Test.

### 29. PA-29

An e-mail step must show Scheduled for (UTC, from the Dataverse trigger) in Amman time as "dd MMM yyyy HH:mm", or the text "not scheduled" when the column is empty. Which expression is correct?

- A. if(empty(triggerOutputs()?['body/cedar_scheduledfor']), 'not scheduled', convertFromUtc(triggerOutputs()?['body/cedar_scheduledfor'], 'Jordan Standard Time', 'dd MMM yyyy HH:mm'))
- B. formatDateTime(addHours(triggerOutputs()?['body/cedar_scheduledfor'], 3), 'dd MMM yyyy HH:mm')
- C. if(equals(triggerOutputs()?['body/cedar_scheduledfor'], ''), 'not scheduled', convertFromUtc(triggerOutputs()?['body/cedar_scheduledfor'], 'Jordan Standard Time', 'dd MMM yyyy HH:mm'))
- D. convertTimeZone(triggerOutputs()?['body/cedar_scheduledfor'], 'UTC', 'Amman', 'dd MMM yyyy HH:mm')

### 30. PA-30

The field control cedar.SitePhotos must let coordinators take a photo with the device camera in the mobile app, and must show its Upload button only when the user can create notes. What should Karim do?

- A. Call context.device.captureImage without declaring anything.
- B. Use context.mode.isControlDisabled to decide whether the user can create notes.
- C. Declare Device.captureImage and WebAPI in feature-usage; call context.utils.getEntityMetadata("annotation"), then context.utils.hasEntityPrivilege before showing Upload.
- D. Use navigator.mediaDevices.getUserMedia directly.

> Checkpoint after PA-30: planned 58.3 minutes elapsed. Actual: ____

### 31. PA-31

The plug-in SiteRiskPlugin must read files from the Azure Storage container site-risk without storing any secret. What should Karim set up?

- A. Put the storage account key in the step's secure configuration.
- B. Create an application user for the storage account in the environment.
- C. Post the request to Service Bus with IServiceEndpointNotificationService and read the files elsewhere.
- D. Sign the assembly, create a user-assigned managed identity with a federated credential for the plug-in, bind a managedidentities record to the assembly, grant Storage Blob Data Reader, and get tokens through IManagedIdentityService.

### 32. PA-32

In the FieldLink Jobs connector, the CreateJob parameter subcontractorCode should show a list of current subcontractors returned by the operation ListSubcontractors (array items, fields code and name). The list changes every week. What should Karim add to the OpenAPI definition?

- A. An enum with the current codes on subcontractorCode.
- B. x-ms-summary: "Subcontractor" on subcontractorCode.
- C. x-ms-visibility: internal with a default value on subcontractorCode.
- D. x-ms-dynamic-values on subcontractorCode with operationId ListSubcontractors, value-collection items, value-path code and value-title name.

### 33. PA-33

The Dispatch Planner's start page reads 120 work orders, then sends one request per work order to read its site (121 requests), and takes 9 seconds to become interactive. What should Karim change first?

- A. Read the sites in one request filtered by the distinct site IDs, keep them in a session cache, and run independent loads in parallel.
- B. Raise the page size to 5,000 rows.
- C. Store the sites in localStorage permanently.
- D. Rewrite the queries in FetchXML.

### 34. PA-34

Cedar Service Assistant (Copilot Studio) must hand dispatch questions to cedar-dispatch-advisor, a Foundry agent owned by another team with its own release cycle. How should Karim connect them?

- A. Add cedar-dispatch-advisor as a child agent of Cedar Service Assistant.
- B. Add it as a connected agent through Add an agent → Microsoft Foundry (preview), after the Foundry agent is enabled for the Activity protocol.
- C. Copy the Foundry agent's instructions into a topic of Cedar Service Assistant.
- D. Call the Foundry agent from a cloud flow with an HTTP action and a stored key.

### 35. PA-35

Releases to Cedar-Prod must wait for Lina's approval and may be deployed only between 18:00 and 20:00 Amman time. Makers have no rights in Cedar-Prod. What should Karim configure in Power Platform Pipelines?

- A. Delegated deployment with a service principal on the production stage (which requires approval), and a pre-deployment step whose handler lets the deployment continue only inside the window.
- B. Give makers the System Customizer role in Cedar-Prod and let them deploy as themselves.
- C. Use the deployment-completed notification to roll back releases deployed outside the window.
- D. Add Cedar-Prod as a target without making it a Managed Environment.

> Checkpoint after PA-35: planned 68 minutes elapsed. Actual: ____

## Section 2: goal set (invoice-status import)

You answer each item once; you can't return to it.

Problem: Every night CedarLedger exports about 180,000 invoice-status changes that a .NET 10 console job (ServiceClient, application user) writes to the standard table Invoice status (cedar_invoicestatus, alternate key cedar_invoicenumber). Last night the job ran four hours, single-threaded, one UpdateRequest per row, and about 300 rows failed because their invoice number didn't exist yet. Goal: finish within 90 minutes, create rows whose invoice number doesn't exist, and never let one bad row stop the others.

### 36. PA-36

Solution: the job sends ExecuteMultipleRequest batches of 500 UpsertRequest messages keyed by cedar_invoicenumber with ContinueOnError = true, from as many parallel tasks as ServiceClient.RecommendedDegreesOfParallelism suggests, and logs the faulted responses. Does the solution meet the goal?

- A. Yes
- B. No

### 37. PA-37

Solution: the job keeps one UpdateRequest per row but runs 64 parallel tasks, and retries a request immediately, up to five times, when it receives a service protection error. Does the solution meet the goal?

- A. Yes
- B. No

### 38. PA-38

Solution: the job sends UpsertMultipleRequest batches of 1,000 rows from eight parallel tasks and logs every batch that fails. Does the solution meet the goal?

- A. Yes
- B. No

> Checkpoint after PA-38: planned 80 minutes elapsed. Actual: ____

## Section 3: case study

You can flag items and review them before you leave this section; after you leave it you can't return. Budget 18 minutes, 2 of them for review.

### CASE-5: REL-034 out-of-hours escalation

Release REL-034, out-of-hours escalation. After two night-time chiller faults at Harbor View Medical Center, Lina Farah, Omar Saleh, Dana Khoury and Karim Aziz agree four requirements on Monday 7 December 2026. (1) Omar's agents need an "Escalate to on-call" command on the Case form. It must ask for confirmation, call the existing custom API cedar_EscalateCase (an action bound to incident with the output EscalationId) and show the result without leaving the form. The API's plug-in queues the page for the Azure Function OnCallPager (func-cedar-ops-prod), which calls the paging provider and logs each failure as the custom event EscalationFailed with the case number to the Application Insights resource appi-cedar-prod. (2) Cedar Night Desk, an internal ASP.NET Core web app for night supervisors, must let the signed-in supervisor ask the published Copilot Studio agent Cedar Service Assistant questions; answers must be limited to what that supervisor may see. (3) CedarReports, the Azure SQL reporting database, keeps a copy of all cases including the escalation columns. Its loader runs every 15 minutes, must also remove deleted cases, and is paused for up to three days during quarterly maintenance. (4) Lina must be told within 10 minutes when more than five pages fail within 15 minutes, with the failed case numbers.

### 39. PA-39

Requirement 1: which call should the command's JavaScript make after the agent confirms?

- A. Xrm.WebApi.updateRecord("incident", id, { cedar_escalate: true })
- B. fetch() to /api/data/v9.2/incidents(<id>)/Microsoft.Dynamics.CRM.cedar_EscalateCase with a bearer token kept in the web resource.
- C. Xrm.WebApi.online.execute(request), where request.getMetadata() returns boundParameter "entity", the parameter entity of type mscrm.incident, operationType 0 and operationName "cedar_EscalateCase".
- D. The same execute call with operationType 1.

### 40. PA-40

Requirement 2: how should Cedar Night Desk talk to Cedar Service Assistant?

- A. The Copilot Studio client library (Microsoft 365 Agents SDK) with the environment ID and the agent's schema name, using a delegated token with CopilotStudio.Copilots.Invoke for the signed-in supervisor.
- B. The same client library with an app-only token from the web app's managed identity.
- C. Call the Dataverse MCP server directly from the web app and skip the agent.
- D. Embed the agent's web chat with authentication set to No authentication.

### 41. PA-41

Requirement 3: how should the CedarReports loader read cases?

- A. Query cases with modifiedon greater than the last run time.
- B. A Service Bus queue endpoint on Create, Update and Delete of incident, read by the loader.
- C. Change tracking with $filter=modifiedon gt <last run> to keep the response small.
- D. Enable Track changes on Case; the first request uses Prefer: odata.track-changes with $select; store the @odata.deltaLink and follow it every 15 minutes, deleting rows reported as $deletedEntity.

### 42. PA-42

Requirement 4: what should Karim set up?

- A. A Power Platform Monitor alert rule on the Cedar Operations app.
- B. An Azure Monitor log search alert on appi-cedar-prod: a KQL query over customEvents named EscalationFailed in the last 15 minutes, threshold more than 5, evaluated every 5 minutes, returning the case numbers, with an action group that notifies Lina.
- C. The Dataverse export to Application Insights with an alert on failed plug-in executions.
- D. A cloud flow that e-mails Lina for each failed page.

### 43. PA-43

Requirement 1: how should the command confirm and report?

- A. window.confirm() before the call and window.alert() with the result.
- B. Xrm.Navigation.openConfirmDialog; when the result has confirmed true, call the API, then formContext.ui.setFormNotification with the EscalationId.
- C. Xrm.Navigation.openAlertDialog to ask, then call the API.
- D. Xrm.Navigation.openForm to a confirmation form, then return to the case.

> Checkpoint after PA-43: planned 96 minutes elapsed. Actual: ____

