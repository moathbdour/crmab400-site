# Cedar AB-400 scenario review: Develop integrations (version 1.0)

28 items, about 56 minutes. Answer closed-book; record confidence as sure, unsure or guess.

### IT-01

FieldLink's receiver is offline every night from 01:00 to 03:00 for maintenance. Every change of a work order's Scheduled for must reach FieldLink, none may be lost, and Sami Nasser's saves must never wait for FieldLink. Which listening option should Karim recommend?

- A. An asynchronous webhook step that posts the execution context to FieldLink's HTTPS endpoint.
- B. A Service Bus queue registered as a service endpoint with an asynchronous step; a consumer forwards the messages to FieldLink when it is available.
- C. A synchronous webhook step, so that a failed delivery is visible to the user at once.
- D. A cloud flow on "When a row is added, modified or deleted" that calls FieldLink with an HTTP action.

### IT-02

The operations wallboard team wants every work order status change as it happens, about 40,000 events an hour at month-end, and must be able to re-read the last 24 hours after a bug fix. No event needs to be confirmed individually. Which option fits best?

- A. A Service Bus topic with one subscription for the wallboard.
- B. An Event Hubs service endpoint (workorder-status) with an asynchronous step; the wallboard reads with its own consumer group.
- C. A webhook to an Azure Function that writes each event into a SQL table the wallboard queries.
- D. Change tracking on cedar_workorder, read every hour.

### IT-03

FieldLink, the audit store and the new billing service each need every work order event. Each consumer works at its own pace, and a message one consumer can't process must be set aside for that consumer only. What should Karim register?

- A. One service endpoint for the Service Bus topic cedar-workorder-events (Designation Type Topic) with one asynchronous step, and one subscription per consumer.
- B. Three Service Bus queues, each registered as its own service endpoint with its own step.
- C. One Event Hubs service endpoint, with a consumer group per consumer.
- D. One webhook per consumer, each with its own asynchronous step.

### IT-04

Dana Khoury asks Karim to register the event hub workorder-status in Cedar-Test with the Plug-in Registration Tool. What does he need, and what does he choose?

- A. A shared access signature policy on the event hub with the Send claim; Register New Service Endpoint with its connection string, Designation Type Event Hub, Message Format JSON.
- B. Register New Web Hook with the event hub's URL and an HttpHeader authorization value holding a SAS token.
- C. The environment's managed identity with the Azure Event Hubs Data Sender role, selected as the endpoint's authorization.
- D. A shared access signature policy with the Listen claim, so Dataverse can check that the hub exists.

### IT-05

The billing service needs a message on the topic only when a work order's cedar_status changes. Karim must not write code for it. What should he register in the Plug-in Registration Tool?

- A. A step on the service endpoint: message Update, table cedar_workorder, filtering attributes cedar_status, PostOperation, asynchronous.
- B. A custom plug-in that compares the pre-image and post-image and calls IServiceEndpointNotificationService when the status changed.
- C. A step on the service endpoint: message Update, table cedar_workorder, filtering attributes cedar_status, PostOperation, synchronous.
- D. A step on the service endpoint for Update with no filtering attributes; billing ignores the messages it doesn't need.

### IT-06

WorkOrderEventPublisherPlugin decides in code whether a change is a reschedule or a cancellation, adds the shared variable cedar.eventKind and then publishes the context to the endpoint "Cedar work order events". Which code publishes it?

- A. var notify = (IServiceEndpointNotificationService)serviceProvider.GetService(typeof(IServiceEndpointNotificationService)); notify.Execute(new EntityReference("serviceendpoint", endpointId), context);
- B. A ServiceBusSender created from a connection string stored in the step's secure configuration, sending the serialised context.
- C. service.Create(new Entity("serviceendpoint") { ["name"] = "Cedar work order events" }) with the context in a text column.
- D. new HttpClient().PostAsync(topicUrl, json) from the plug-in with a SAS token.

### IT-07

The asynchronous publisher plug-in also creates a cedar_integrationlog row before it calls Execute. After the topic was unreachable for ten minutes, one change produced four log rows but only one message. What should Karim change?

- A. Remove everything from the plug-in except preparing the context and posting; let the consumer log, using OperationId to recognise a repeated post.
- B. Register the plug-in synchronously, so the post isn't retried.
- C. Catch the exception around Execute and return normally, so the system job succeeds the first time.
- D. Create the log row only when context.Depth is 1.

### IT-08

Finance owns a credit-check service in Azure that must approve every new work order for a customer on credit hold before it's saved; a refusal must cancel the save. The rule must stay in finance's service. Which option meets this?

- A. A synchronous webhook step on Create of cedar_workorder; a non-2xx answer from the service cancels the operation.
- B. A Service Bus queue endpoint with an asynchronous step; the service deletes refused work orders.
- C. A cloud flow on row added that checks the service and sets the work order to Cancelled.
- D. An Event Hubs endpoint read by the credit-check service.

### IT-09

Cedar's reporting database needs all work order and site changes every hour, including deleted rows. Karim's first draft queries rows with modifiedon greater than the last run. What should he use instead?

- A. Change tracking: request the entity set with Prefer: odata.track-changes, store the @odata.deltaLink, and follow it next hour; deletions arrive as $deletedEntity entries.
- B. Keep the modifiedon query and add a second query for statecode eq 1 to find deleted rows.
- C. The Event Hubs stream workorder-status, written into the reporting database.
- D. Read the audit history of both tables every hour.

### IT-10

To keep the copy small, Karim adds $filter=statecode eq 0 to his change-tracking request. Dataverse now refuses the request. What is the correct fix?

- A. Remove $filter; select columns with $select only and filter in the consumer.
- B. Replace $filter with $top=5000.
- C. Enable Track changes on the table again and repeat the request.
- D. Send the same filter as FetchXML with a delta token.

### IT-11

Karim adds the alternate key cedar_FieldLinkIdKey to Work order and, one minute later, his import that writes to cedar_workorders(cedar_fieldlinkid='FL-77310') fails because the key can't be used. What should he do?

- A. Wait until the key's index status is Active; if it's Failed, find the duplicate values, fix them and reactivate the key.
- B. Publish all customizations again.
- C. Add column security to cedar_fieldlinkid so the key is protected.
- D. Write the key value in double quotes in the address.

### IT-12 (choose 2)

Karim needs to identify the work order with FieldLink ID FL-77310 without knowing its GUID. Which TWO are correct?

- A. Web API address cedar_workorders(cedar_fieldlinkid='FL-77310').
- B. SDK new Entity("cedar_workorder", "cedar_fieldlinkid", "FL-77310"), which stores the value in KeyAttributes.
- C. PATCH cedar_workorders?$filter=cedar_fieldlinkid eq 'FL-77310'.
- D. SDK new Entity("cedar_workorder") { ["cedar_fieldlinkid"] = "FL-77310" } passed to UpdateRequest.
- E. Web API address cedar_workorders(cedar_fieldlinkid=FL-77310).

### IT-13

A repair job sets FieldLink status on existing work orders by FieldLink ID. If no work order has that ID, nothing may be created; the job should count it as "missing". Which request should Karim send?

- A. PATCH cedar_workorders(cedar_fieldlinkid='FL-…') with If-Match: *; 404 means missing.
- B. PATCH cedar_workorders(cedar_fieldlinkid='FL-…') without headers; Dataverse creates or updates as needed.
- C. PATCH cedar_workorders(cedar_fieldlinkid='FL-…') with If-None-Match: *.
- D. An UpsertRequest; if RecordCreated is true, delete the new row and count it as missing.

### IT-14

A partner file lists sites by site code; some exist in Cedar and some don't. A run may be repeated after a time-out, and Lina Farah wants to know how many sites were created. How should Karim write each row with the SDK?

- A. An UpsertRequest whose Target is new Entity("cedar_site", "cedar_sitecode", code) with only the data columns in Attributes; count RecordCreated.
- B. Retrieve by key, then Create or Update depending on the result.
- C. A CreateRequest; on the duplicate-key error, send an UpdateRequest.
- D. An UpsertRequest with the site code also in Attributes, to be sure the key is saved.

### IT-15

The FieldLink Jobs connector uses OAuth 2.0 with FieldLink's authorization server. After Karim selected "Update to unique redirect URL", new connections fail at FieldLink's sign-in page with a redirect URI error. What should he do?

- A. Add the connector's redirect URL, shown on the Security tab, to FieldLink's client registration; each environment's connector has its own.
- B. Grant admin consent for the Dynamics CRM user_impersonation permission.
- C. Switch the connector to API key authentication with FieldLink's client secret as the key.
- D. Add https://global.consent.azure-apim.net/redirect to FieldLink's registration and keep the global redirect.

### IT-16

A partner sends an OpenAPI 3.0.1 file of 1.4 MB that describes 160 operations; Cedar needs three of them. How should Karim create the connector?

- A. Import the file as it is from a URL; the portal converts it.
- B. Produce an OpenAPI 2.0 definition with the three operations, below 1 MB, and import that file.
- C. Split the 3.0.1 file into two files of 0.7 MB and import both.
- D. Choose Import from GitHub and point it at the partner's file.

### IT-17

An independent-publisher connector for a weather service already exists in the microsoft/PowerPlatformConnectors repository. Karim wants a custom copy in CedarConnectors that he can change. What is the fastest supported way?

- A. Create it from the repository's connector (Import from GitHub, or pac connector create with its apiDefinition.swagger.json and apiProperties.json).
- B. Import an OpenAPI file from the URL of the connector's README.md.
- C. Rebuild the connector from blank, operation by operation.
- D. Export the certified connector from another tenant's solution and import it.

### IT-18

Cedar's SLA API is published in Azure API Management. Lina wants a connector for it in Cedar-Test with the least effort, and later changes should be pushed from the same place. What should Karim do?

- A. In API Management, open APIs, choose Power Platform, then Create a connector for the API and the Cedar-Test environment; use Update a connector for later changes.
- B. Export the OpenAPI from API Management, edit it by hand and import it into a blank connector.
- C. Call the API from every flow with the HTTP action and the subscription key in the URL.
- D. Create a Logic App that calls the API and let flows call the Logic App.

### IT-19

Karim's Azure Function behind a custom connector must know which user called it, to check the delegated scope Sla.Read per user. How should the function be secured?

- A. Authorization level Function, with the function key as the connector's API key.
- B. App Service authentication with Microsoft Entra ID in front of an anonymous HTTP trigger; the connector uses OAuth 2.0 with Entra ID; the function reads X-MS-CLIENT-PRINCIPAL.
- C. Authorization level Admin, with the master key stored in a connection reference.
- D. Pass the user's email address as a query parameter from the flow.

### IT-20

In the FieldLink Jobs connector, the Subcontractor parameter of CreateJob should be a drop-down filled from the ListSubcontractors operation, so it follows changes in FieldLink. Which extension should Karim add?

- A. x-ms-dynamic-values with operationId ListSubcontractors, value-collection, value-path and value-title.
- B. An enum with today's subcontractor references.
- C. x-ms-visibility: important.
- D. x-ms-summary: "Subcontractor".

### IT-21

FieldLink moves job creation from /v1/jobs to /v2/jobs with the same body. Forty flows use the CreateJob action, and Lina doesn't want any of them changed. What should Karim add to the connector?

- A. A Route request policy on CreateJob with the new path /v2/jobs.
- B. Custom code that rewrites the request URL to /v2/jobs before calling SendAsync.
- C. A new operation CreateJobV2 and an update of the forty flows.
- D. A Set host URL policy pointing at the v2 host.

### IT-22

Every call of the FieldLink Jobs connector must carry the header X-Cedar-Environment with the value the user entered as a connection parameter environmentName. What is the simplest way?

- A. A Set HTTP header policy with the value @connectionParameters('environmentName') and an empty operation list, so it applies to all operations.
- B. An internal header parameter with a fixed default value on every operation.
- C. Custom code that copies the connection parameter into the header.
- D. An environment variable referenced inside the policy.

### IT-23

FieldLink's availability operation returns an object such as {"slots": {"2026-12-07T08:00Z": "free", …}}. Flows need an array of free slots in Amman local time, without past slots. Where should Karim put this logic?

- A. Custom code in the connector: a Script class deriving from ScriptBase whose ExecuteAsync forwards the request with SendAsync and reshapes the response.
- B. The Convert object to array policy template.
- C. Expressions in each of the flows that use the action.
- D. A Set property policy template that adds a localTime property.

### IT-24 (choose 2)

Which TWO statements about custom code in a custom connector are true?

- A. The script must finish within 2 minutes.
- B. The script file must be smaller than 1 MB, and a connector has one script.
- C. The script can reference any NuGet package it needs.
- D. The script can write traces to Application Insights through an ILogger.
- E. The script can reach an on-premises API through the data gateway.

## CASE-4: LedgerLine billing feed

Cedar Service is moving invoicing to LedgerLine, a fictional billing provider. On Thursday 3 December 2026 Lina Farah, Dana Khoury and Karim Aziz agree four requirements. (1) Each work order that becomes Completed must reach LedgerLine within minutes; LedgerLine's receiver is down for up to six hours every Sunday, and each completion must produce exactly one invoice (LedgerLine's API accepts an Idempotency-Key header). (2) LedgerLine's reporting job keeps a copy of Cedar's sites through change tracking; it was switched off for nine days in November. (3) LedgerLine sends back an invoice number per FieldLink ID; the function that writes it must never create work orders and must report, per message, whether a work order was updated. (4) Finance flows call LedgerLine's invoice API through a custom connector; LedgerLine's API accepts only the OAuth 2.0 client credentials grant, finance wants to see which user triggered each call, and no secret may be stored in flows or connections. Constraints: Cedar-Prod uses the topic cedar-workorder-events; only FieldLink Sync and the billing function may write work order columns owned by integrations.

### CI-1 (choose 2)

Case study CASE-4, requirement 1. Which TWO parts make the billing feed meet the requirement?

- A. A service endpoint for a Service Bus queue (or a billing subscription on cedar-workorder-events) with an asynchronous step on Update of cedar_workorder filtered on cedar_status.
- B. A Service Bus-triggered function that sends each message to LedgerLine with the message id as idempotency key, lets the broker redeliver on transient failures and dead-letters permanent ones.
- C. An asynchronous webhook step that posts directly to LedgerLine's receiver.
- D. A synchronous plug-in that calls LedgerLine and cancels the completion if LedgerLine is down.
- E. An Event Hubs endpoint read by LedgerLine once a day.

### CI-2

Case study CASE-4, requirement 2. LedgerLine's reporting job, which follows a change-tracking delta link for cedar_site, was switched off for nine days. What must it do now?

- A. Load all sites again with a new change-tracking request, replace its copy, and continue with the new delta link.
- B. Send the old delta link with $filter=modifiedon ge the last run date.
- C. Ask Dana to raise the change-tracking retention to 30 days and send the old delta link again.
- D. Start a new delta link today and keep the existing copy as it is.

### CI-3

Case study CASE-4, requirement 3. How should the function write each invoice number?

- A. PATCH cedar_workorders(cedar_fieldlinkid='…') with If-Match: * and the invoice column; 204 is updated, 404 is an unknown work order and nothing is created.
- B. PATCH cedar_workorders(cedar_fieldlinkid='…') with Prefer: return=representation; 201 means created, 200 updated.
- C. An UpsertRequest per invoice; delete rows where RecordCreated is true.
- D. A POST to cedar_workorders with the FieldLink ID and invoice number.

### CI-4

Case study CASE-4, requirement 4. How should the connector and LedgerLine access be built?

- A. An Azure Function behind the connector: App Service authentication with Entra ID and a connector using OAuth 2.0 with Entra ID; the function reads the caller from X-MS-CLIENT-PRINCIPAL, logs it, and gets LedgerLine's client secret from Key Vault with its managed identity to call LedgerLine with client credentials.
- B. A custom connector to LedgerLine configured with the OAuth 2.0 client credentials grant.
- C. A connector with API key authentication whose key is LedgerLine's client secret, in one connection shared with the finance team.
- D. A function with authorization level Function; each flow passes the user's name in a header.

