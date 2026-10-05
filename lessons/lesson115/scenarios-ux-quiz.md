# Cedar AB-400 scenario review: Extend the user experience (version 1.0)

30 items, about 60 minutes. Answer closed-book; record confidence as sure, unsure or guess.

### UX-01

Lina Farah wants Priority hidden on the Case form whenever Category is Inquiry. Priority appears twice on the form: in the header and in the General tab. Karim's OnChange handler calls formContext.getControl("prioritycode").setVisible(false), but the header copy stays visible. What should he change?

- A. Loop over formContext.getAttribute("prioritycode").controls and call setVisible(false) on each control.
- B. Use Xrm.Page.getControl("prioritycode") so the call applies to the whole page.
- C. Call formContext.getAttribute("prioritycode").setVisible(false).
- D. Find the header element with document.querySelector and set style.display to "none".

### UX-02

A Case form handler must show a warning when Priority is High. Cedar's agents use the app in English and Arabic, and a maker may rename the option labels next year. Which condition should Karim write?

- A. formContext.getAttribute("prioritycode").getText() === "High"
- B. formContext.getAttribute("prioritycode").getValue() === 1
- C. formContext.getControl("prioritycode").getValue() === "High"
- D. Xrm.Page.data.entity.attributes.get("prioritycode").getValue() === "1"

### UX-03

When Omar Saleh picks a Customer on a case, the Site lookup must show only sites that belong to that customer. How should Karim implement the filter?

- A. In the form designer, add an OnChange handler on Site that removes sites of other customers from the results.
- B. In the OnLoad handler, call formContext.getControl("cedar_site").addPreSearch(filterSites); inside filterSites, call addCustomFilter with a filter on the customer's account id.
- C. In the OnLoad handler, call addCustomFilter once on the Site control, outside any PreSearch handler.
- D. Add a business rule that clears Site whenever Customer changes.

### UX-04

Lina wants Dana Khoury's makers to be able to switch off the urgent-case banner handler on the Case form without a code release. The handler reads formContext from the execution context. How should Karim register it?

- A. In the form designer: Events → On Load → add cedar.caseTriage.onLoad from the library, with "Pass execution context as first parameter" selected.
- B. In the app designer, enable the async OnLoad handlers setting for Cedar Operations.
- C. From another OnLoad handler in code: formContext.ui.addOnLoad(() => cedar.caseTriage.onLoad()).
- D. In the form designer without passing the execution context, and read the form with Xrm.Page inside the handler.

### UX-05

When Omar picks a Site, the Case form must check that the site belongs to the case's customer. The script has the site's GUID in siteId. Which request is correct and efficient?

- A. Xrm.WebApi.retrieveRecord("cedar_sites", siteId)
- B. Xrm.WebApi.retrieveRecord("cedar_site", siteId, "?$select=_cedar_account_value")
- C. Xrm.WebApi.retrieveRecord("cedar_site", siteId, "?$select=cedar_account")
- D. fetch(clientUrl + "/api/data/v9.2/cedar_sites") and search the result for siteId

### UX-06

A Case form command must call Cedar's custom API cedar_AssessCase (an unbound action that returns RiskLevel) and show the result. How should the script call it?

- A. Xrm.WebApi.online.execute with a request whose getMetadata returns operationName "cedar_AssessCase" and operationType 0; read RiskLevel from the JSON body of the response.
- B. Xrm.WebApi.retrieveMultipleRecords("cedar_assesscase", "?$select=risklevel")
- C. Xrm.WebApi.online.execute with operationType 1, because the API returns a value.
- D. A fetch to the Web API with a token for Cedar's application user, whose client secret is stored in the web resource, so the call runs with elevated rights.

### UX-07 (choose 2)

Lina's team wants an "Escalate selected" button on the Cases main grid. It must appear only when 1 to 10 cases are selected, ask for confirmation showing the count, and report failures per case. Which TWO should Karim configure in the command designer?

- A. Action: Run JavaScript, calling cedar.caseCommands.escalateSelected from its library, with the parameter SelectedControlSelectedItemIds.
- B. Visibility: Show on condition from formula, CountRows(Self.Selected.AllItems) >= 1 && CountRows(Self.Selected.AllItems) <= 10.
- C. Visibility formula Not(IsBlank(Self.Selected.Item)).
- D. A classic RibbonDiffXml display rule, edited later in the command designer.
- E. An OnLoad script that finds the button in the page and hides it with CSS when too many rows are selected.

### UX-08

While Omar edits an unsaved case, he must be able to open the other open case at the same site, read it, and come back with one click without losing his unsaved changes. Which call should the form use?

- A. Xrm.Navigation.openForm({ entityName: "incident", entityId: otherId })
- B. Xrm.Navigation.navigateTo({ pageType: "entityrecord", entityName: "incident", entityId: otherId }, { target: 2, position: 2, width: { value: 40, unit: "%" } })
- C. window.open(clientUrl + "/main.aspx?etn=incident&id=" + otherId)
- D. Xrm.Navigation.openAlertDialog with the other case's title and description.

### UX-09

After Omar saves a Critical fault case, the reminder "Call the on-call technician" must stay visible while he moves on to the queue list, with a button that opens the on-call schedule. Which API fits?

- A. formContext.ui.setFormNotification("Call the on-call technician", "WARNING", "oncall")
- B. Xrm.App.addGlobalNotification with type 2, a level and an action, keeping the returned id to clear it later.
- C. formContext.getControl("prioritycode").setNotification("Call the on-call technician", "oncall")
- D. Xrm.Navigation.openAlertDialog in an OnPostSave handler.

### UX-10

After a change to C-22 (the FieldLink status timeline), the Work order form flickers, CPU stays high and the record is marked as unsaved as soon as it opens. The new code calls notifyOutputChanged() at the end of updateView "so the form always has the latest value". What should Karim change?

- A. Call notifyOutputChanged only in response to a user action or a real internal change (for example in the click handler), never unconditionally in updateView.
- B. Move the notifyOutputChanged call into getOutputs.
- C. Assign the new value to context.parameters.status.raw inside updateView instead.
- D. Increase the manifest version so the host reloads the bundle.

### UX-11

Karim adds a property to the C-25 dispatch cards component that makers bind to an order value column. Makers must be able to choose a Whole number, Decimal or Currency column. What should the manifest contain?

- A. Three optional properties, orderWhole, orderDecimal and orderCurrency, one per column type.
- B. A type-group listing Whole.None, Decimal and Currency, and a property with of-type-group pointing to it.
- C. One property with of-type="Decimal"; the platform converts the other column types.
- D. A feature-usage element that declares Utility, so the component can read any column type.

### UX-12

Sami Nasser clicks "Mark as In progress" in C-22. The new status must go through the Work order form, so that business rules run and the normal save applies. How should the component implement this?

- A. In the click handler, store the new value and call notifyOutputChanged(); getOutputs returns { status: this.value } for the bound property.
- B. In the click handler, call context.webAPI.updateRecord on the work order with the new status.
- C. Declare status with usage="input" and return it from getOutputs.
- D. In updateView, set the value through formContext.getAttribute("cedar_fieldlinkstatus").

### UX-13

Karim changed C-22's TypeScript, rebuilt it, packed CedarServiceCore and imported it into Cedar-Test. The import succeeded, but users still see the old behaviour, even after clearing their browser cache. What is the most likely cause, and the fix?

- A. The customizations weren't published; run Publish all customizations in Cedar-Test.
- B. The manifest version wasn't increased; increase it (for example with pac pcf version --strategy manifest), rebuild and deploy again.
- C. pac pcf push should be used against Cedar-Test so the bundle is replaced directly.
- D. The bundle was built in production mode; switch PcfBuildMode to development.

### UX-14

C-22 offers "Take site photo" with context.device.captureImage. Since the last release, the component works in the mobile app but fails to load at all in the browser. What should Karim change?

- A. Declare <uses-feature name="Device.captureImage" required="false" /> and show the button only when the host provides the method.
- B. Remove the feature-usage element; the typings already include device.
- C. Enable external-service-usage so the browser can reach the camera.
- D. Build a separate canvas app for site photos.

### UX-15

Before showing the photo button, C-22 must find out whether the user can create notes (annotation) at all. Which approach is reliable?

- A. Call context.utils.hasEntityPrivilege("annotation", 1, 0) as the first statement in init.
- B. Await context.utils.getEntityMetadata("annotation"), then call context.utils.hasEntityPrivilege("annotation", 1, 0), and still handle a failed createRecord.
- C. Check context.mode.isControlDisabled.
- D. Retrieve the user's security roles with context.webAPI and compare role names with a list in the code.

### UX-16

C-23 Cedar Dispatch Planner shows job titles from Office 365 Users. Karim added the data source with --connection-id, using his own connection in Cedar-Dev. After the managed solution is imported into Cedar-Test, the calls fail. What should he have done?

- A. pa app add data-source --connector shared_office365users --connection-ref cedar_Office365Users --solution-id <CedarServiceCore id>, so each environment maps the reference to its own connection.
- B. Share his Cedar-Dev connection with all planner users in Cedar-Test.
- C. Put the connection id in an environment variable and read it at runtime.
- D. Call Microsoft Graph directly from the app with a client secret in the app configuration.

### UX-17

Sami's dispatchers must see in C-23 exactly the work orders they can see in Cedar Operations, no more. How should Karim add the work order data?

- A. pa app add data-source --connector dataverse --table cedar_workorder; requests run as the signed-in user, so Dataverse security roles and business units apply.
- B. Create a Dataverse connection with a service account, add it with --connection-id, and filter rows in React by the user's team.
- C. Query with FetchXML through the generated service so the Open dispatch view's filter is reused.
- D. Build a custom connector that calls the Dataverse Web API with an application user.

### UX-18

C-23's grid header must show the user-language display names of three Work order columns. Which approach should Karim use?

- A. Call the generated service's getMetadata with the three columns in schema.columns, read Attributes[].DisplayName.UserLocalizedLabel.Label, and cache the result.
- B. Keep an English label constant per column in the code.
- C. Use getMetadata to read the option list of cedar_fieldlinkstatus and derive the column labels from it.
- D. Call getMetadata for every row as it renders.

### UX-19

C-23's "My work orders" filter must compare ownerid with the current user's id as Dataverse stores it. Which approach is correct?

- A. Use getContext().user.objectId.
- B. Add the WhoAmI function with pa app add dataverse-api --api-name WhoAmI and use UserId from its response.
- C. Read the user's email from Office 365 Users and filter work orders on the owner's email address.
- D. Call the bound action AddToQueue with the user's id.

### UX-20

Sami wants a "Request dispatch" button in C-23 that sends a work order to FieldLink. Today the logic is in a nightly scheduled flow that isn't in a solution, and pa app list-flows doesn't show it. What should Karim do?

- A. Create a solution-aware instant cloud flow with the Power Apps trigger (it can call a child flow shared with the nightly flow), then add it with pa app add flow.
- B. Add the nightly flow to CedarServiceCore; once it is solution-aware, pa app add flow accepts it.
- C. Change the nightly flow to an automated flow triggered when a work order is modified, and set a column from the app.
- D. Call FieldLink from the app with fetch, using the FieldLink API key from the app settings.

### UX-21

C-23 must send a dispatcher's question to the published Copilot Studio agent Cedar Service Assistant and show the answer in the planner. Which approach should Karim use?

- A. Add the Microsoft Copilot Studio connector as a data source and call ExecuteCopilotAsyncV2 (Execute Agent and wait) with the message and agent name; render the reply as plain text.
- B. Call ExecuteCopilot, the original operation, because it is simpler.
- C. Embed the agent's demo website in an iframe inside the planner.
- D. Render the agent's reply with dangerouslySetInnerHTML so its formatting is kept.

### UX-22

C-23 reads approved invoices from a SharePoint list. The site and the list are different in Cedar-Dev, Cedar-Test and Cedar-Prod, and Lina wants one build for all three. How should Karim add the data source?

- A. With pa app add data-source for the SharePoint connector, passing --dataset "@envvar:cedar_InvoiceSite" and --table "@envvar:cedar_InvoiceList".
- B. Detect the environment with getContext() and pick the site URL from a constant map in the code.
- C. Store the site URL in a Secret environment variable.
- D. Ask each environment admin to edit power.config.json after the solution is imported.

### UX-23

After adding Application Insights to C-23, the planner works in Cedar-Test but no telemetry arrives. The browser console shows a Content Security Policy violation for the connect-src directive when the SDK sends data to the Application Insights ingestion endpoint. What should Dana change?

- A. Add the ingestion origin to script-src in the environment's code app CSP.
- B. Add the ingestion origin from the violation message to connect-src in the environment's code app CSP; because connect-src defaults to 'none', the value replaces the default.
- C. Turn off Enforce content security policy for code apps in Cedar-Test.
- D. Add https://*.azure.com to connect-src so future Azure services work too.

### UX-24 (choose 2)

Lina wants C-23's load time per session and its slow data requests in Application Insights, with one build for all environments. Which TWO are part of the solution?

- A. Call setConfig({ logger: { logMetric } }) once from the app and forward the sessionLoadSummary and networkRequest metrics to Application Insights, for example with trackEvent.
- B. Read the Application Insights connection string for the current environment from cedar_DispatchSettings in Dataverse after sign-in.
- C. Reference the connection string with @envvar: in power.config.json.
- D. Enable the Power Platform admin center data export to Application Insights to see each session within minutes.
- E. Load the Application Insights SDK from a CDN script tag in index.html.

### UX-25 (choose 2)

Rafi Hanna needs to use C-23 in Cedar-Prod. He has a Microsoft 365 E3 licence and the Basic User role. Cedar's governance rule GOV-05 excludes security groups from app sharing and caps sharing at 20 users. Which TWO steps are required?

- A. Give Rafi a Power Apps Premium licence (or enable pay-as-you-go for Cedar-Prod).
- B. Share the app with Rafi as an individual user and add him to the Dispatch Team – Cedar team, which holds the planner's Dataverse role.
- C. Share the app with the Cedar Dispatch security group.
- D. Run pa app push from Karim's laptop into Cedar-Prod with Rafi as co-owner.
- E. Give Rafi the System Customizer role so the app can read environment variables.

### UX-26

A planner screen is slow on Karim's machine. In the browser developer tools he wants to separate connector calls from Dataverse calls. Which Network filter strings should he use?

- A. apihub.net for connector calls and dynamics.com for Dataverse calls.
- B. api/data for both kinds of calls.
- C. None: connector calls appear only in the plug-in trace log.
- D. None: he should wait for the next daily Power Platform Monitor report.

## CASE-2: Cedar Dispatch Planner 2.0 go-live

Version 2.0 of C-23 Cedar Dispatch Planner is in Cedar-Test before its move to Cedar-Prod. Lina Farah wants the planner embedded in the Cedar Operations model-driven app, so dispatchers stop switching apps. During user acceptance testing on Friday 27 November 2026, Sami Nasser reports four problems. (1) The planner opens on its own URL, but inside Cedar Operations it is blank and the browser console shows a "Refused to frame" message. (2) Two dispatchers say the app never opens for them, and Application Insights has no sessions from them. (3) With 120 open work orders, the board takes 9 seconds to become usable and DevTools shows 121 Dataverse requests: one for the work orders and one per work order to resolve its site name. (4) The release to Cedar-Prod must follow Cedar's ALM rules: managed solutions through the Cedar Release Pipeline with approvals; Karim has no maker rights in Prod; connection references and settings differ per environment. Dana Khoury owns the environment settings, and every change to an environment-wide security setting needs a change record.

### CU-1

Case study CASE-2, problem 1. What should Dana change so the planner shows inside Cedar Operations?

- A. Add Cedar-Test's organization URL (https://<cedar-test>.crm.dynamics.com) to frame-ancestors in the code app CSP, after reading the current directives so no other directive is reset.
- B. Add the organization URL to connect-src.
- C. Turn off CSP enforcement for code apps in Cedar-Test.
- D. Add https://*.dynamics.com to frame-ancestors so one value works for every environment.

### CU-2

Case study CASE-2, problem 2. Where should Karim start?

- A. Power Platform admin center Monitor for Power Apps: the planner's app open success rate and error codes, then sharing, licence and data policy for those two users.
- B. Query Application Insights customEvents for the two users' sessions.
- C. Ask the two users to record a DevTools performance profile of the board.
- D. Turn on the plug-in trace log in Cedar-Test.

### CU-3 (choose 2)

Case study CASE-2, problem 3. Which TWO changes address the cause?

- A. Resolve site names in batches with an OR filter on the validated site ids, and cache the results for the session.
- B. Start the independent loads (work orders, subcontractors, settings) in parallel, each catching its own errors.
- C. Increase the page size to 5,000 so all work orders arrive in one request.
- D. Rewrite the query in FetchXML with a link-entity so site names come in the same call.
- E. Store resolved site names and site contacts in localStorage permanently.

### CU-4

Case study CASE-2, problem 4. How should planner 2.0 reach Cedar-Prod?

- A. The Cedar Release Pipeline deploys managed CedarServiceCore (which contains the code app) to Cedar-Prod after approval, with a deployment settings file for connection references and environment variable values.
- B. The CI job runs pa app push --solution-id <CedarServiceCore id> against Cedar-Prod with the cedar-ci-dev service principal.
- C. Connect Cedar-Prod to Power Platform Git integration and sync the app from the repository.
- D. Export CedarServiceCore as unmanaged from Cedar-Test and import it into Cedar-Prod.

