# Cedar AB-400 scenario review: Extend Microsoft Power Platform (version 1.0)

34 items, about 68 minutes. Answer closed-book; record confidence as sure, unsure or guess.

### EX-01

Lina Farah wants every attempt to save a work order with Scheduled for in the past refused, whether it comes from the form, FieldLink Sync or an import. The rule needs only the value the caller submits. Where should Karim register the synchronous plug-in step so that a refusal costs the least?

- A. PreOperation, because the rule must run inside the database transaction.
- B. PreValidation, so the operation is cancelled before the transaction starts.
- C. PostOperation in asynchronous mode, so the save stays fast.
- D. A cloud flow on "When a row is added or modified" that deletes the row when the date is in the past.

### EX-02

When a work order becomes Completed, FieldLink must be told through its REST API. The call takes 1–4 seconds, and FieldLink is sometimes unavailable for an hour. Sami Nasser's save must never fail or wait because of FieldLink. What should Karim do?

- A. Call FieldLink from a synchronous PostOperation step and catch every exception so the save always succeeds.
- B. Call FieldLink from an asynchronous PostOperation step filtered on the status column, with an idempotent request and the failure visible in system jobs.
- C. Call FieldLink from a synchronous PreOperation step with an HttpClient timeout of 2 seconds.
- D. Keep the synchronous step and ask Dana Khoury to raise the plug-in time-out for Cedar-Prod.

### EX-03

Step R1 runs as Cedar Automation ("Run in User's Context"). When FieldLink Sync rejects a work order, R1 creates a follow-up task. Lina wants the task description to name the identity that actually made the update. Which value should the plug-in use?

- A. context.UserId
- B. context.InitiatingUserId
- C. The UserId returned by a WhoAmIRequest sent through CreateOrganizationService(context.UserId).
- D. context.ParentContext.UserId, read from the outermost parent.

### EX-04

A synchronous PreValidation step on Update of cedar_workorder must refuse a change to Site once the FieldLink status is Sent. Most callers send only the Site, not the status. How should the plug-in get the status?

- A. Read cedar_fieldlinkstatus from the Target entity.
- B. Register a pre-image with alias pre that contains only cedar_fieldlinkstatus, and read it from PreEntityImages.
- C. Register a post-image with cedar_fieldlinkstatus and read it from PostEntityImages.
- D. Register a pre-image with all columns so later rules can use it too.

### EX-05

A work order that was already sent to FieldLink must not be deleted, even by an administrator's mistake. Karim registers a synchronous PreValidation step on Delete of cedar_workorder. How does the plug-in read the work order's FieldLink status?

- A. Cast InputParameters["Target"] to Entity and read cedar_fieldlinkstatus.
- B. Register a pre-image on the Delete step with cedar_fieldlinkstatus and read it; the Target is only an EntityReference.
- C. Register a post-image on the Delete step and read the status from it.
- D. Use an asynchronous PostOperation step that retrieves the row and re-creates it if it had been sent.

### EX-06

When a work order is created without a Service line, it should take the Service line of its Site. Karim's synchronous PreOperation step on Create has the Site's ID from the Target. What should the code do?

- A. Retrieve the Site with a ColumnSet containing only cedar_servicesrequired, then set target["cedar_serviceline"].
- B. In a PostOperation step, call service.Update on the new work order with the Service line.
- C. Call the Dataverse Web API with HttpClient to read the Site, then set the Target value.
- D. Create a ServiceClient from a connection string kept in the step's secure configuration and read the Site with it.

### EX-07

The case close guard (C-10) must block resolving a case while open work orders exist. Omar Saleh can't read every work order of his cases. How should the plug-in count them?

- A. Query with CreateOrganizationService(context.UserId) so Omar's security is respected.
- B. Query with CreateOrganizationService(null), ColumnSet(false) and TopCount 51, and show only the number in the error.
- C. Give the Service Agent – Cedar role Organization-level Read on work orders so the count is complete.
- D. Retrieve the work orders with ColumnSet(true) as SYSTEM and list their names and subcontractors in the error.

### EX-08

A synchronous plug-in on Update of account checks the credit limit. Omar's account form has become slow, and the trace log shows the step running on every save. The logic reads only creditlimit. What should Karim change first?

- A. Set the step's filtering attributes to creditlimit.
- B. Change the step to asynchronous mode.
- C. Return at the top of Execute when context.Depth > 1.
- D. Set the filtering attributes to accountid and creditlimit.

### EX-09

Karim designs cedar_CheckSiteAccess: a read-only check on one Site that the Dispatch Planner and a cloud flow both call. Nobody else should be able to register logic that changes its result. Which definition fits?

- A. A function, bound to cedar_site, Allowed Custom Processing Step Type None.
- B. An action, bound to cedar_site, Allowed Custom Processing Step Type None.
- C. A custom process action built in the classic workflow designer.
- D. An action, bound to cedar_site, Allowed Custom Processing Step Type Sync and Async.

### EX-10

Karim writes the plug-in for cedar_RequestDispatch, an action bound to cedar_workorder that returns Result ("queued" or "already-requested"). How is the plug-in connected and how does it return Result?

- A. Register a synchronous PreOperation step on cedar_RequestDispatch and put Result into the Target.
- B. Register the assembly without a step, set the custom API's Plugin Type, check MessageName and Stage 30, and set context.OutputParameters["Result"].
- C. Throw InvalidPluginExecutionException with the Result text, so the caller always sees it.
- D. Register an asynchronous PostOperation step that writes Result to a work order column for the caller to read later.

### EX-11

Step G1 reads its grace period (in minutes) from the step's secure configuration in Cedar-Dev. After the managed solution is imported into Cedar-Test, G1 traces "grace period missing, using default". What should Karim do?

- A. Move the grace period to the unsecure configuration (or an environment variable): secure configuration isn't included in solution exports, and the value isn't secret.
- B. Export the solution again with the "Include secure configuration" option.
- C. Give Karim the System Administrator role in Cedar-Test so he can read the secure configuration.
- D. Run pac plugin push against Cedar-Test to register the step again with the value.

### EX-12

FieldLink reports "subcontractor arrived" for a work order. Cedar wants flows, asynchronous plug-ins and endpoints to subscribe to this fact, and nothing in Dataverse should be able to make FieldLink's call fail. How should Karim define cedar_OnSubcontractorArrived?

- A. A custom API with a main operation plug-in that updates the work order, so subscribers react to the change.
- B. A custom API with no plug-in type, Allowed Custom Processing Step Type Async Only, request parameters only, and a catalog assignment in a category.
- C. A custom API with Sync and Async and a response property Accepted, so FieldLink knows the event arrived.
- D. A custom API with no plug-in type, assigned directly to the root catalog Cedar Service.

### EX-13 (choose 2)

Cedar.Service.Plugins must read a secret from kv-cedar-test with Power Platform managed identity. Which TWO are part of the setup?

- A. Sign the plug-in assembly with a certificate and add a federated identity credential to the managed identity, with the documented issuer and the version 2 subject identifier.
- B. Create a managedidentities record (version 2) bound to the plug-in assembly, and give the identity the Key Vault Secrets User role on kv-cedar-test.
- C. Store the managed identity's client secret in the step's secure configuration.
- D. Create an application user in Cedar-Test for the managed identity's client ID.
- E. Give the managed identity the Key Vault Administrator role so it can read any secret it needs later.

### EX-14

The FieldLink Gateway updates the Site of an existing work order through the Web API. It must never create a work order by accident. Which request is right?

- A. PATCH /api/data/v9.2/cedar_workorders(<id>) with If-Match: * and body { "cedar_Site@odata.bind": "/cedar_sites(<site id>)" }
- B. PATCH /api/data/v9.2/cedar_workorders(<id>) with body { "cedar_Site@odata.bind": "/cedar_sites(<site id>)" } and no If-Match header
- C. PATCH /api/data/v9.2/cedar_workorders(<id>) with If-Match: * and body { "cedar_site": "<site id>" }
- D. POST /api/data/v9.2/cedar_workorders(<id>) with If-Match: * and body { "cedar_Site@odata.bind": "/cedar_sites(<site id>)" }

### EX-15

A .NET console tool must call the bound function cedar_CheckDispatch for one work order through IOrganizationService. Which call is right?

- A. service.Execute(new OrganizationRequest("cedar_CheckDispatch") { ["Target"] = new EntityReference("cedar_workorder", id) })
- B. service.Retrieve("cedar_CheckDispatch", id, new ColumnSet("CanDispatch"))
- C. service.RetrieveMultiple(new FetchExpression("cedar_CheckDispatch"))
- D. An ExecuteMultipleRequest containing one cedar_CheckDispatch request for the work order.

### EX-16

Each night FieldLink Sync updates about 900 work orders. The rows are independent, plug-ins may refuse individual rows, and the report needs a verdict per row. Which approach fits best?

- A. ExecuteMultiple batches of up to 1,000 UpdateRequest items with ContinueOnError = true and ReturnResponses = true.
- B. One UpdateMultiple request with all 900 rows, because it is the fastest message.
- C. One ExecuteTransaction request with all 900 updates, so the night's work is consistent.
- D. 900 single Update requests on 100 threads with the affinity cookie left on.

### EX-17 (choose 2)

The FieldLink Gateway acquires a token with the client credentials flow, but every Dataverse request returns 401 Unauthorized. Which TWO are likely causes?

- A. The token was requested for a different audience, for example another environment's URL.
- B. The token was acquired once at start-up and has expired.
- C. The FieldLink Sync application user lacks a privilege in its security role.
- D. The app registration has no delegated user_impersonation permission.
- E. FieldLink Sync has exceeded the service protection limits.

### EX-18

The Dispatch Planner starts cedar_RequestDispatch for up to 300 work orders when Sami confirms a weekly plan. Sami must not wait for the work, and Dana wants Cedar notified when each request finishes. Which approach fits?

- A. Send each request with Prefer: respond-async; keep the Location status monitor URL, and use a callback URL or the OnBackgroundOperationComplete event for completion.
- B. Register an asynchronous PostOperation step on Update of the work order to do the dispatch work.
- C. Call the API synchronously and raise the planner's HTTP time-out to 10 minutes.
- D. Call the API synchronously and set the client's retry count to 10 so failed calls are repeated.

### EX-19

Karim's Python tool must create 2,000 Site rows in Cedar-Test each night as an application identity, with as few requests as possible. Which code fits the Dataverse SDK for Python?

- A. DataverseClient(url, CertificateCredential(tenant_id, client_id, cert_path)) and client.records.create("cedar_site", rows) with rows as lists of up to 1,000 dictionaries.
- B. The same client, calling client.records.create("cedar_site", row) once per dictionary in a loop.
- C. The same client, sending all 2,000 creates in one batch changeset.
- D. DataverseClient(url, DeviceCodeCredential()) so Dana signs in when the job starts.

### EX-20

A cloud flow calls an HTTP-triggered function that applies a FieldLink file. The work takes 4–6 minutes. functionTimeout is 10 minutes, but the flow's call fails after about 4 minutes. What should Karim change?

- A. Start a Durable Functions orchestration from the HTTP function, return 202 Accepted with the status URL, and let the flow follow the status until it completes.
- B. Raise functionTimeout to 30 minutes.
- C. Move the function app to a plan with a longer HTTP limit.
- D. Return 200 immediately and continue the work on a background thread in the same invocation.

### EX-21

func-cedar-ops-dev uses the user-assigned identity id-cedar-ops-dev and requests a token for https://cedar-dev.crm.dynamics.com/.default. Locally it works with Karim's sign-in; in Azure every Dataverse call fails with "The user is not a member of the organization". What fixes it?

- A. Create an application user in Cedar-Dev for the identity's application (client) ID and give it a least-privilege security role.
- B. Grant the identity the Dynamics CRM user_impersonation API permission in Microsoft Entra ID with admin consent.
- C. Switch to a client secret stored in the function app's settings.
- D. Give the identity the Contributor role on rg-cedar-dev.

### EX-22

A flow's email subject must show a work order's Scheduled for in Amman local time, or the word "unscheduled" when the column is empty (it is then missing from the trigger output). Which expression is right?

- A. if(empty(triggerBody()?['cedar_scheduledfor']), 'unscheduled', convertFromUtc(triggerBody()?['cedar_scheduledfor'], 'Jordan Standard Time', 'ddd dd MMM HH:mm'))
- B. convertFromUtc(triggerBody()['cedar_scheduledfor'], 'Jordan Standard Time', 'ddd dd MMM HH:mm')
- C. if(equals(triggerBody()?['cedar_scheduledfor'], ''), 'unscheduled', convertFromUtc(triggerBody()?['cedar_scheduledfor'], 'Jordan Standard Time', 'ddd dd MMM HH:mm'))
- D. if(empty(triggerBody()?['cedar_scheduledfor']), 'unscheduled', formatDateTime(addHours(triggerBody()?['cedar_scheduledfor'], 3), 'ddd dd MMM HH:mm'))

### EX-23 (choose 2)

The flow "Cedar – Dispatch ready work orders" must send Dana an alert when any of its update actions fails or times out, and the run history must then show the run as Failed. Which TWO elements are needed?

- A. A scope Try with the update actions, and a scope Catch whose run after is "has failed" and "has timed out" on Try.
- B. A Terminate action with status Failed at the end of Catch, after the alert.
- C. An Exponential interval retry policy on every update action.
- D. A Terminate action inside the Apply to each, right after the failed update.
- E. A scope Catch whose run after is "has failed" only.

### EX-24

A flow on "When a row is added, modified or deleted" (Modified, cedar_workorder) must run only when the FieldLink status becomes Rejected (104570005). Today it runs on every work order update. What should Karim configure?

- A. Select columns cedar_fieldlinkstatus and Filter rows cedar_fieldlinkstatus eq 104570005.
- B. Keep the trigger and add a Condition as the first action that ends the run unless the status is Rejected.
- C. A trigger condition that compares the previous status with the new status.
- D. Select columns cedar_workorderid, cedar_fieldlinkstatus.

### EX-25

The child flow "Cedar – Report flow failure" posts to Teams and sends email. The parent flow can't be saved: the child "only supports embedded connections". What should Karim do?

- A. On the child's details page, edit Run only users and choose "Use this connection" for the Teams and Office 365 Outlook connections.
- B. Pass the parent's Teams connection to the child as an input parameter.
- C. Move both flows out of the solution so the child can use the caller's connections.
- D. Replace Run a Child Flow with an HTTP action that calls the child's trigger URL.

### EX-26

The connection reference cedar_DataverseAutomation must use a service principal in Cedar-Test, so Cedar's flows no longer depend on Karim's account. What must exist in Cedar-Test?

- A. An app registration with a certificate, an application user for it with the Flow Automation – Cedar role, and a "Connect with service principal" connection mapped to the reference in the deployment settings file.
- B. An app registration with the Dynamics CRM user_impersonation permission and admin consent, used by a "Connect with service principal" connection.
- C. Karim's OAuth connection shared with Dana, so the flows keep running when he is away.
- D. An application user with the System Administrator role, so no flow ever fails on privileges.

### EX-27

Cedar's Foundry agent cedar-dispatch-advisor reads work orders through the Dataverse MCP server. Every response now stops with an mcp_approval_request item. Lina wants read-only tools to run without approval and every other tool to stay unavailable. What should Karim change in the agent's MCP tool?

- A. List only the read-only tools in allowed_tools and set require_approval to never for them.
- B. Set require_approval to never for all tools on the server.
- C. Add "never ask for approval" to the agent's instructions.
- D. Point the tool at the server's /sse endpoint instead.

### EX-28

In Cedar-Dev, the Copilot Studio agent C-24 is connected to the Foundry agent cedar-dispatch-advisor (preview connection). The first test fails with "HTTP error with code 400 … endpoint does not support activity". What should Karim do?

- A. Enable the Activity protocol on the agent endpoint with the REST API or an SDK, sending the full protocol list that keeps responses.
- B. Give the maker the Foundry User role on the project.
- C. Re-create the agent in the classic Foundry portal.
- D. PATCH the endpoint's protocol configuration with activity only.

### EX-29

Sami asks Cedar's advisor about WO-1001 in Teams. The answer must use only work order data Sami is allowed to read. The advisor's host (C-57) calls the Dataverse Ask API through C-58. How should the call be authenticated?

- A. The host exchanges Sami's token on behalf of him and calls the Ask API with that delegated token.
- B. The MCP server C-56 calls the Ask API with the agent's own identity.
- C. An application user with the System Administrator role calls the Ask API, and the model is told what Sami may see.
- D. A service identity calls the Ask API, and the instructions tell the model to leave out rows Sami can't read.

### EX-30

A nightly Azure Function with a managed identity should ask the Copilot Studio agent C-24 for a summary of open complaints and email it to Lina. Karim planned to use the Copilot Studio client library. What should he do?

- A. Use Direct Line (or a flow with the Microsoft Copilot Studio connector): the client library doesn't support service principal tokens.
- B. Grant the managed identity an application permission for CopilotStudio.Copilots.Invoke.
- C. Store Dana's refresh token in the function and call the client library as her.
- D. Configure the client with the agent's display name instead of its schema name.

## CASE-3: FieldLink month-end catch-up

Before the December release, Dana Khoury rehearses month-end in Cedar-Test on Tuesday 1 December 2026. Two function apps are involved: the operations health check (C-44, timer-triggered, schedule 0 30 2 * * *, meant to run at 02:30 Amman time) and the FieldLink sync (C-45). Dana reports four problems. (1) The health check runs at 05:30 Amman time, and on the night Dataverse was briefly unavailable it failed and nothing ran again until the next night. (2) A catch-up run of 40,000 work order updates by FieldLink Sync starts receiving HTTP 429 responses with Retry-After values of 20 to 90 seconds after about four minutes. (3) In the same run, Cedar's work order guard refuses 12 updates because those work orders are Completed and locked; Lina Farah wants all other rows applied and a list of the 12 with reasons. (4) A Power Automate flow starts the catch-up through an HTTP-triggered function and must tell Sami Nasser the outcome, but the run takes about 25 minutes. Constraints: only FieldLink Sync may write work orders; no secrets in app settings; FieldLink Sync's service protection budget is shared with its other jobs; Amman is UTC+3 all year.

### CE-1 (choose 2)

Case study CASE-3, problem 1. Which TWO changes fix the health check's schedule and its missed runs?

- A. Change the schedule to 0 30 23 * * * (UTC), which is 02:30 in Amman.
- B. Let the timer function only put a message on a queue, and do the checks in a queue-triggered function with retries and a poison queue.
- C. Set RunOnStartup to true so a missed check runs whenever the app restarts.
- D. Throw an exception when IsPastDue is true so the timer retries the run.
- E. Change the schedule to the five-field expression 30 2 * * *.

### CE-2 (choose 2)

Case study CASE-3, problem 2. Which TWO changes handle the 429 responses correctly?

- A. Rely on ServiceClient's built-in handling: it waits for Retry-After and resends; set MaxRetryCount and RetryPauseTime, and end the run with a specific outcome when retries are used up.
- B. Limit parallel requests to RecommendedDegreesOfParallelism and let Retry-After pace the run instead of starting at full speed.
- C. Wrap every ServiceClient call in Cedar's own loop that retries five times immediately.
- D. Create a second application user for the catch-up run to double the limits.
- E. Ask Microsoft support to raise the service protection limits for Cedar-Test.

### CE-3

Case study CASE-3, problem 3. How should the catch-up run apply the updates?

- A. ExecuteMultiple with ContinueOnError = true, recording each fault with its RequestIndex, and listing the refused rows without retrying them.
- B. UpdateMultiple in chunks of 1,000 rows.
- C. ExecuteTransaction requests of 1,000 updates each.
- D. Send the requests with the BypassBusinessLogicExecution header so the lock plug-in can't refuse them.

### CE-4

Case study CASE-3, problem 4. How should the flow start the catch-up and report its outcome to Sami?

- A. The HTTP function starts an orchestration and returns 202 with a status URL; the flow checks the status in a Do until loop with a Delay and a Timeout longer than the run, then reports the final status.
- B. Set the flow's HTTP action time-out to 30 minutes so it waits for the function's answer.
- C. Add a 30-minute Delay after the call and then tell Sami the catch-up finished.
- D. Do the whole catch-up inside the orchestrator function so it can run as long as it needs.

