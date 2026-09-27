# Designing the Enterprise AI Architecture

Once we understand the components of an enterprise AI system, we have to decide how to put them together. This is where the conversation moves from what the technology can do to what the business needs, what we are willing to let the system affect, and who will be responsible for it.

I have worked in environments where decisions happen quickly and the information is incomplete. That is often the reality in a growing software company. The business needs something, the team has an idea, and waiting for perfect certainty is not a useful option. I have also worked in environments where a change needs careful coordination because the consequences extend well beyond the application being changed.

Architecture has to accommodate both. It should help people make informed decisions at an appropriate pace. A design process that cannot keep up with the work will struggle to influence it. A process that approves everything without understanding the consequences is not doing much useful work either.

With AI, the pressure to get moving can be particularly strong. People see demonstrations, hear what competitors are doing, and worry about being left behind. That urgency is understandable. It also makes it easy to select a product before defining the problem or enable consumption before establishing who owns the bill.

My preference is to establish enough structure that teams can experiment without creating avoidable surprises. Give them access, a clear scope, and a budget. Make the boundaries understandable and enforceable. Then let them build and use what they learn to shape the next decision.

## Start With the Work

A model comparison can be an interesting technical exercise. It becomes an architecture decision only when we understand what the model needs to accomplish.

Consider the maintenance-impact application from the previous chapter. An employee wants to know which production applications depend on databases scheduled for maintenance. The business outcome is a more accurate impact assessment produced with less manual investigation.

That description gives us something to design around. The application needs current maintenance records, useful dependency information, and a way to connect its conclusions to evidence. It also needs to distinguish a confirmed relationship from one that cannot be established with the available data.

We should understand how the work happens today. An engineer may already use a combination of change records, monitoring tools, documentation, and conversations with application owners. Some of that process may be slow because information is scattered. Some may be slow because the underlying records are incomplete. Those are different problems, and AI will not resolve both simply by providing a conversational interface.

The people doing the work can help identify the difference. They know which sources they trust, which records are usually stale, and where an apparently simple question becomes complicated. Bringing them into the design early can save considerable effort later.

A useful first version might gather evidence and draft an assessment for an engineer to review. That is a meaningful improvement even if the system does not independently approve the maintenance or notify every affected team. We do not have to automate the entire process to make part of it better.

## Establish the Boundaries Before Enabling Consumption

I want cost guardrails in place before people begin consuming the technology. An experiment should have an owner, an approved scope, and a defined amount of spending. We should also know what happens when it approaches or reaches its limits.

A budget alert provides visibility, but it may not stop consumption. Where spending needs to be constrained, the design needs controls that can actually restrict work. Depending on the environment, those might include quotas, request limits, concurrency limits, or a controlled mechanism for disabling access. The behavior should be understood before someone leaves a test running over the weekend.

Costs can also exist outside model requests. An experiment might create persistent infrastructure, storage, search services, or provisioned capacity. Stopping the application does not necessarily stop those charges. The environment needs an owner and a lifecycle, including cleanup when the work ends.

Governance belongs at this starting point too. Teams need to know which services they can use, what data can enter the environment, and whether the system can perform actions. They also need a clear process for requesting something outside those boundaries.

This does not require a complete governance program before anyone writes a line of code. It requires enough agreement to make the experiment legitimate and bounded. A small evaluation using approved test data can have a straightforward path. Connecting an agent to production infrastructure deserves more attention.

The Cloud Center of Excellence and FinOps are under my leadership, so I see these decisions as part of enabling the work. We should make the approved environment useful enough that teams can focus on their problem. The following chapters will develop the governance and cost controls in detail. Here, the architectural point is that they shape the environment from the beginning.

## Assign Service Ownership Before Opening Access

A purchased enterprise AI platform still needs someone to operate it. The provider may run the infrastructure, but the customer has responsibilities inside the service: access, configuration, approved integrations, consumption settings, support, and escalation.

Those responsibilities can be easy to overlook during implementation. One team configures single sign-on. Another establishes provisioning. FinOps sets up reporting. The service becomes available, and then a user needs a different entitlement or a department requests a higher spending limit. The implementation team has finished its work, but the operating model is still being negotiated.

That is a gap we need to close deliberately. Configuring the service does not automatically establish who will administer it next month.

A named service owner should be accountable for the overall offering, including support arrangements, vendor escalation, lifecycle decisions, and coordination across teams. That person does not have to perform every administrative task. Identity specialists can operate authentication and provisioning, while a designated operations team handles routine tenant administration and service requests.

Financial responsibilities also need to be explicit. FinOps can establish allocation, reporting, and spending controls without becoming the default support queue for account changes. Security and governance functions can establish requirements without taking responsibility for daily fulfillment.

The same principle applies across an employee-facing AI assistant, a coding service, and a cloud platform used to build applications. Their administrative tasks differ, but each needs a clear owner and an operating team.

Someone eventually has to adjust the setting, answer the ticket, or call the vendor. We should decide who that is before the first request gets forwarded through six teams.

## Connect Access to Approval and Chargeback

In our organization, we created a central cost center for AI. I own that cost center as part of my responsibility for AI FinOps, and our plan is to charge costs back to the business units consuming the services.

That arrangement creates two related financial responsibilities. The central owner needs visibility and control over aggregate AI spending. The receiving cost center owner needs to approve the expense their organization will carry. Centralizing the invoice does not eliminate the need for that business approval.

The access process should preserve the relationship between the request, the user or workload, and the receiving cost center. If we lose that connection during provisioning, we make chargeback harder than it needs to be. Reconstructing ownership from a billing export is an excellent way to spend time nobody budgeted for.

For individual licenses, attribution may be tied to assigned users. Metered services may require usage records associated with projects, applications, or organizational identifiers. Shared platform costs need an agreed allocation method. These are different charging patterns, and the method should be understandable before a cost center owner approves access.

We also need to handle change. People transfer departments, applications change owners, and experiments end. Attribution and entitlements should follow those events rather than remain attached to the original request indefinitely.

This is our organizational approach, not a requirement that every enterprise use a central AI cost center. The broader architectural requirement is to connect financial approval, access, consumption, and ownership in a way the organization can operate.

## Make the Service Catalog the Front Door

I see an approved AI service request following a familiar pattern. A user selects an offering from the service catalog, identifies the business purpose and receiving cost center, and obtains the appropriate approval. Fulfillment then applies the approved access and configuration.

That is closely related to requesting a virtual machine through a catalog and using infrastructure as code to deliver it. The request captures what is needed, the approval establishes authority to proceed, and automation performs a repeatable set of actions.

For a SaaS AI offering, fulfillment might assign an identity group and provision an account or entitlement. Where supported, the System for Cross-domain Identity Management, usually called SCIM, can automate user provisioning and deprovisioning. The exact behavior needs to be checked for the service, including how removing access affects licenses and retained content.

For a cloud AI offering, fulfillment might provision a project or environment, configure a workload identity, apply approved access policies, establish supported consumption controls, and connect usage reporting. The user experience can remain consistent even when the work underneath is different.

The catalog should contain approved offerings, with enough information for users and approvers to understand what they are requesting. That includes the intended use, applicable data boundaries, charging method, and support arrangements. Teams also need a route to request a new capability that is not yet in the catalog.

Financial approval should not silently authorize every technical capability. A cost center owner agreeing to pay for access does not automatically approve a new data source, a different model, or an integration that can change production systems. Those changes may require additional review under the established policy.

Automation makes the process faster when those decisions are clear. It also makes revocation, recertification, and ownership changes more manageable. The aim is a complete service lifecycle, rather than an efficient way to create accounts that nobody later removes.

## Define What an Acceptable Result Looks Like

A system needs an outcome we can evaluate. “It gives good answers” is difficult to design around because people will interpret it differently.

For the maintenance application, an acceptable result might identify the relevant applications, explain the dependency relationships, cite the supporting records, and identify gaps requiring review. Missing a critical application could be more consequential than including an extra application for an engineer to inspect. That difference should influence evaluation.

We also need examples where the appropriate result is to stop short of a conclusion. If the dependency service is unavailable, the application should not quietly interpret that as evidence that no applications are affected. If records disagree, it should make the disagreement visible.

These cases belong in the design and evaluation set. A few successful demonstrations tell us that the application can work under those conditions. They do not establish how it behaves when information is missing, permissions differ, or a dependency returns an unexpected result.

The users reviewing the output should help define acceptance. A response can read beautifully while omitting the detail an engineer needs to make a decision. Their feedback connects model behavior to the actual work.

## Choose How Much Autonomy the Task Needs

Not every AI application needs an agent. Some tasks can be handled by one model call. Others benefit from retrieval or a predefined workflow. An agent becomes useful when the system needs discretion about which steps to take based on what it discovers.

That discretion has consequences for testing and support. A workflow that always checks the maintenance record, queries dependencies, and generates a draft assessment has a relatively clear execution path. An agent that selects sources and tools dynamically may handle a wider range of situations, but it also introduces more possible paths.

I would begin the maintenance application with the simplest pattern that meets the requirement. If the necessary queries and sequence are already understood, there may be little benefit in asking a model to invent that sequence each time.

We can add flexibility when evidence shows that it improves the work. Perhaps different application types require different investigations, or the system needs to follow relationships that cannot be predicted in advance. Those are reasons to consider an agent, provided its authority and execution limits remain clear.

Autonomy should be earned through demonstrated usefulness and acceptable behavior. It does not need to be a requirement just because it looks good in the product description.

## Separate Information From Authority

An application that helps an engineer assess maintenance does not automatically need permission to modify the maintenance schedule. Reading a record, drafting a recommendation, and executing a change are separate capabilities.

Keeping those boundaries explicit makes the system easier to reason about. The initial application might have read access to approved sources and return a draft for review. A later version could prepare a change to a record, while a separate controlled service performs the write after approval.

If actions are introduced, the execution layer should validate the operation, target, parameters, and authority. Permissions need to be enforced by the systems controlling the resources. Instructions to the model can guide behavior, but they cannot replace those checks.

Human approval also needs useful information. The reviewer should understand what will change, where it will change, and the relevant consequences. An approval should apply to the action actually performed. If the agent changes the target or scope afterward, the earlier approval may no longer be sufficient.

This is familiar operational discipline applied to a different execution mechanism. We should preserve the controls that make consequential actions understandable and accountable.

## Decide What to Share

As teams begin building, common needs appear. Several applications may need access to the same model providers, usage reporting, identity patterns, or deployment tooling. Providing those capabilities centrally can reduce duplicated effort and improve consistency.

The challenge is deciding where shared infrastructure ends and application responsibility begins. A platform team can provide model access and telemetry conventions. It cannot define a correct maintenance-impact assessment without input from the people who understand that work.

I believe in self-service automation because routine tasks should not require repeated manual intervention. A team deploying an approved application pattern should be able to obtain the necessary infrastructure and controls through a repeatable process. That can include managed identities, access policies, consumption limits, monitoring, and deployment configuration.

Application teams still need responsibility for their data choices, business logic, evaluation criteria, and user experience. A common platform should support different workloads without forcing them into an identical design.

The people expected to use the platform need a voice in its development. If a capability is technically available but difficult to consume, that friction affects delivery. Platform adoption gives us useful feedback about whether we have made the work easier.

## Place the Workload Deliberately

Deployment location should follow requirements rather than preference. The application, retrieval services, source systems, and model endpoint may run in different environments. Each connection affects identity, data handling, latency, availability, and cost.

A managed model service can reduce operational work around serving and capacity. Self-hosting can provide more control over parts of the environment while creating responsibility for deployment, scaling, patching, and recovery. Neither approach removes the need to understand the whole request path.

The maintenance example may benefit from accessing current dependency records directly while retrieving supporting documentation from an index. That design avoids relying on a periodic copy for information whose freshness is critical. It also creates a dependency on the live source service during the request.

The choice should be explicit. If the live service is unavailable, the application needs a defined response. If a cached result is allowed, the user may need to see its age and limitations. The appropriate behavior depends on the business decision the answer will support.

These are the tradeoffs that turn a collection of components into an architecture. We should be able to explain why each important dependency exists and what happens when it is unavailable.

## Design Failure and Recovery Together

Failure handling should be part of the normal design discussion. Services time out, records are incomplete, permissions change, and actions sometimes complete without returning confirmation.

The response needs to fit the failure. Retrying a read can be reasonable. Retrying a write without checking whether it already succeeded may create duplicate work or repeat a disruptive action. Execution interfaces should support safe retries or a way to determine the status of a prior request where possible.

Fallbacks also require evaluation. An alternate model may behave differently or have different approval for processing the data. A fallback that keeps requests moving while violating a data restriction has not met the requirement.

Sometimes the correct degraded behavior is a narrower result. The maintenance application might provide the records it could confirm and clearly identify the unavailable source. In other cases, it should stop and direct the user to the established manual process.

Recovery planning extends beyond an individual failed request. The design needs to identify the state and dependencies required to restore service after a larger disruption. Application configuration, access policies, retrieval indexes, and incomplete agent workflows may all matter.

Recovery objectives should come from the business process. We need to understand how long the application can be unavailable, what information can be reconstructed, and how essential work continues while recovery happens. Testing those assumptions provides more confidence than a recovery document nobody has exercised.

## Give the Operating Team Evidence

When I moved alerting into our own tools during an MSP transition, the objective was to prevent a gap as responsibilities changed. Taking ownership of a service requires a practical way to see what is happening and respond.

An AI application needs that same attention before it becomes something people depend on. The support team should be able to follow the request across its important dependencies and identify where execution failed or quality degraded.

That does not require retaining every piece of sensitive content indefinitely. It requires deliberate choices about the evidence needed, who can access it, and how long it is retained. Request identifiers, component versions, timings, tool status, and appropriate evaluation results can all contribute to investigation.

The design should also identify who responds. The platform team may own model access, while the application team owns retrieval behavior and the business outcome. Escalation needs to connect those responsibilities without leaving the user to coordinate them.

A green infrastructure dashboard is helpful. It is more helpful when the team can explain why a technically successful request produced an unusable result.

## Plan for Change

Models, prompts, tools, source systems, and retrieval indexes will change. Any of those changes can affect the application’s behavior.

The deployed system therefore needs an identifiable configuration. When investigating a result, the team should be able to establish which relevant versions were in use. Releases should include evaluation appropriate to the change and a practical way to recover if the result is unacceptable.

Rollback can be more complicated than selecting an earlier model. A retrieval change may have altered an index, and a tool change may have affected state in another system. The recovery approach should account for those relationships.

I would also record the reasoning behind important architectural decisions. That can be concise: the constraint, the choice, the tradeoff, and the conditions that would justify revisiting it. Future engineers should not have to reconstruct the decision from calendar invitations and somebody’s memory.

A design can be appropriate today and need revision later. Recording the assumptions makes that revision easier to discuss.

## Make the First Version Supportable

The first production version does not need every capability we can imagine. It needs a clear purpose, bounded authority, acceptable results, and an operating model appropriate to the workload.

For the maintenance application, that might mean a read-only service that gathers current evidence and drafts an assessment for an engineer. It would operate within an established budget, preserve source permissions, identify incomplete information, and provide enough telemetry for support. It would also have a defined recovery approach and a manual alternative when unavailable.

For a purchased AI service, the first supported offering might establish approved access through the catalog, financial attribution, identity integration, routine administration, and an escalation path. The provider may supply the application, but the enterprise still needs to deliver a complete service to its users.

Both can be useful starting points. We can evaluate them, learn from their use, and decide whether additional capabilities are justified.

This is how I want architecture to support experimentation. Establish the boundaries, build something useful, and follow through on what the results teach us. When something fails, the team should be able to explain what happened, what changed, and how the improvement was tested.

Before we choose more technology, we need the agreements that make that work possible. The next chapter looks at governance: who makes the decisions, what boundaries apply, and how teams move from an idea to a supported service without spending the entire journey waiting for permission.
