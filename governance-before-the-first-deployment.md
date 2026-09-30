# Governance Before the First Deployment

Someone wants access to an AI service. Their manager is on board, the department has a reasonable use for it, and there is money to pay for it. This should be a service request we know how to fulfill.

Then the questions start. Nobody is quite sure which information the employee can upload. The service has connectors, but nobody has decided which ones can be enabled. The team that configured single sign-on is getting the request, even though it never agreed to administer the platform.

Now a request for access has become an impromptu architecture meeting.

I have spent enough time building and operating platforms to know that these gaps will eventually find someone. Usually, they find the person trying to get work done or the engineer holding the ticket. Both would benefit from a few decisions being made earlier.

That is where governance becomes useful. It establishes what people can use, the boundaries around that use, and who has the authority to approve something different. Done well, it gives teams a way to move forward without having to negotiate the operating model every time someone needs an account.

I encourage experimentation, and I understand the pressure around AI. People see what competitors are announcing and worry about being left behind. We should give that enthusiasm somewhere productive to go, with enough structure that we can support what comes out of it.

## Start With the Decisions Blocking the Work

Governance can become a large subject very quickly. We can discuss security, data, procurement, models, spending, intellectual property, employee use, and operational risk. Those are legitimate concerns. A team trying to evaluate a useful idea still needs to know what it can do this week.

I would start with the decisions required to let that team proceed. Establish an approved environment, identify the information it can use, assign an owner, and define the spending limits. Be clear about whether the system can only produce information or can also perform actions. Give the team a review point and a path for taking a successful experiment further.

That is enough to create a useful starting boundary. It does not settle every future question, and it does not need to.

The requirements should be specific enough to guide a decision. "Use AI responsibly" is difficult to disagree with, but it will not help an engineer decide whether to connect a repository containing internal documents. The engineer needs to know whether that connection is permitted, whose approval is required, and what controls apply.

A service offering might permit employees to work with defined categories of internal information while limiting connectors to a reviewed set. A development environment might provide approved model endpoints and test data with no access to production systems. Those are boundaries people can understand and administrators can work to enforce.

The details will change between services. We need to explain them clearly enough that approval of one capability does not get interpreted as approval of everything carrying the same product name.

## Decide Who Can Say Yes

Several teams may contribute to an AI decision. That is reasonable. Trouble starts when everyone can identify a concern but nobody is responsible for bringing the decision to a conclusion.

The cost center owner can approve an expense. The data owner can assess whether the proposed use is appropriate for the information involved. Identity specialists can establish how access works, and security can evaluate the relevant controls. The service owner needs to bring those requirements together into something the organization can deliver and operate.

Those responsibilities should be defined before they are needed for a routine request. Otherwise, the requester becomes a project manager for an account provisioning exercise.

In our organization, we created a central cost center for AI. I own it through my responsibility for AI FinOps, and our plan is to charge the consuming business units for their use. That gives me financial accountability. It does not make me the authority for every data source someone wants to connect or every action an agent might perform.

If a department wants an agent to modify an operational system, the owner of that system needs to participate in the decision. Being willing to pay for the agent does not establish authority over everything it can reach.

A governance group can resolve questions that cross these responsibilities and establish standards for others to follow. It should also delegate routine decisions. Access to an already approved offering should not have to wait for the next monthly meeting. The meeting calendar is a lousy provisioning dependency.

## Approve What People Will Actually Use

Approving a product name leaves a lot open to interpretation. An AI platform might support chat, file uploads, external connectors, code execution, and actions in other systems. Those capabilities can have different consequences even when they appear in the same interface.

The approved offering should describe the configuration and use that were reviewed. Users need to understand which features are available, what information they can provide, and which integrations are supported. Administrators need to know what they can change without additional review.

Model choices belong in that discussion when the service exposes them. Approval of a platform should not automatically mean every model it makes available is suitable for every workload. The service owner needs a way to manage those choices and evaluate material changes.

This also creates an ongoing responsibility. A provider can introduce new functionality without the enterprise buying a new product. The application inventory may look exactly the same while the service gains access to more information or the ability to perform actions.

Someone needs to review those changes and determine whether the current approval still applies. That work belongs in service ownership, alongside support and lifecycle management. It cannot depend on an administrator happening to notice a release announcement during lunch.

## Give People Somewhere to Experiment

If we want employees to experiment within the approved boundaries, we should provide an environment that makes that practical. Access, identity, data restrictions, and consumption controls should be established before the team starts building.

The environment should match the question being explored. A team evaluating document classification might use approved sample data and write results to an isolated location. It may not need a connection to the full source repository or permission to update business records.

That narrower scope helps the team move quickly. It also makes the results easier to interpret because the experiment has a defined purpose. The team is trying to establish whether an approach is useful, what it costs, or how much work an integration requires.

I would give the experiment an owner and a review date. At that point, the team can close it, extend it for a reason, or propose a supported implementation. All three can be appropriate outcomes. Discovering that an idea does not perform well enough can save the organization from making a larger investment in it.

The review date also gives us a reason to clean up. Experiments have a habit of leaving behind infrastructure, licenses, storage, and access. They can continue generating costs long after everyone has moved on to something more interesting.

Trying something should be easy enough to encourage. Finishing it should include dealing with what we created.

## Match the Review to the Consequences

A drafting assistant and an agent authorized to change production systems should not receive identical treatment. The review needs to reflect the information involved, the authority granted, and what happens when the result is wrong.

The same principle applies within a single industry. A manufacturing company can experiment quickly with an isolated internal use case while taking more time over a capability connected to production operations. A software company can move quickly on one feature while applying extensive validation to a service its customers depend on.

The workload matters more than the label.

A useful intake process captures enough information to make that distinction. It should identify the users, data sources, intended outputs, and connected systems. It should explain what the application can change and how people will review its work.

That last part deserves attention. "A human will review it" sounds reassuring until we discover that the person receives several hundred outputs a day and has no supporting evidence. We should understand what the reviewer will see, whether they can recognize an error, and whether they have time to make a meaningful decision.

Established review paths can make this predictable. Teams should be able to recognize a straightforward request and understand why a more consequential one requires additional work. That allows specialists to focus on decisions that need their judgment instead of processing every request as though it were new.

## Make Data Rules Understandable

Employees need to understand how data rules apply to the information they use every day. Broad policy language becomes more useful when it is accompanied by recognizable examples.

Source code, customer records, operational logs, financial information, and internal documentation can carry different restrictions. The approved offering should explain which uses are permitted and where additional review is required.

The service's configuration and data-handling arrangements need to support those decisions. Relevant questions about retention, access, and processing should be reviewed by the appropriate functions. We should not expect an employee to resolve them independently or infer the answer from a product page.

Connecting a source deserves particular attention. Giving a service access to a repository can expose far more information than a user entering a single question. The design needs to preserve appropriate permissions, and someone with authority over the source needs to approve the connection.

Outputs require care too. A generated summary can still contain sensitive information. Rewriting a document does not remove the restrictions that apply to its contents.

Where technical controls can enforce a requirement, we should use them. Where they cannot, the limitation needs to be explicit so the organization can decide whether a narrower scope, additional oversight, or another service is necessary. Telling users to be careful should not be our only plan for a problem we already understand.

## Put the Decisions Into the Service Catalog

The service catalog is where these decisions become something people can use. An employee should be able to find an approved offering, understand its supported uses, data boundaries, and support arrangements, and submit a request with the information needed for fulfillment. The request should also identify who approves the expense. The next chapter covers how we preserve that financial relationship.

This is familiar work for a platform team. We already use catalogs and automation to deliver infrastructure with approved configurations. The same approach can provision AI access, assign identity groups, apply available controls, and connect reporting.

A routine license request should follow the established path. A new integration or expanded permission needs a visible route for review. Users should understand the distinction before they begin configuring the service.

The catalog should also support changes and retirement. An efficient account-creation process is useful. It is even better when we remember that accounts sometimes need to go away.

## Leave Room for Exceptions

Approved offerings will not cover every legitimate requirement. Teams need a way to explain why the current options are insufficient and propose an alternative.

An exception should identify the business need, additional risk, proposed controls, and responsible owner. The decision should come from the people authorized to accept that risk, with a review or expiration point.

Without that last part, a temporary exception can become permanent through inattention. After enough of those, the written standard and the operating environment start describing different organizations.

Some exceptions will reveal a recurring need. If several teams request the same capability and it can be supported, we may need to add an offering or revise the standard. Others should remain limited, and some should be declined.

The explanation matters. People do not have to like every decision, but they should be able to understand the reason and the conditions that might change it. A process that produces only "no" without useful context teaches people very little about how to succeed next time.

## Recognize When the Experiment Has Become a Service

An experiment can become important before anyone formally calls it production. Someone finds it useful, shares it with a colleague, and soon a team is incorporating the output into its daily work. That is a good reason to review it. The work may have value, and people are developing a dependency.

The transition needs an owner, support arrangements, appropriate evaluation, access controls, financial attribution, and recovery expectations. The people expected to operate the service need enough documentation and visibility to do their jobs. A pilot that works for a small team does not automatically establish capacity for the entire company or approval for additional data sources.

In September 2026, the U.S. administration and several leading AI companies announced a voluntary agreement covering internal controls, independent audits, and board-level oversight. For enterprise leaders, the practical question is whether those commitments produce controls we can evaluate and trust in our own environments. [AP's report on the voluntary agreement](https://apnews.com/article/trump-ai-anthropic-musk-595796511f110fc006cca0d01329733e)

I would apply the same expectation inside the company. If we say an application only uses approved data, we should be able to demonstrate how that restriction works. If a consequential action requires approval, the system should prevent it from proceeding without that approval. A policy gives us the expectation. Evidence helps us decide whether we can depend on it.

Much of that evidence can come from work the team already needs to perform: evaluation results, deployment configuration, access reviews, monitoring, and recovery exercises. Governance should connect that evidence to the decision to operate the service. We should avoid making engineers write a second account of the same work merely to satisfy a different template.

Approval also needs to keep pace with the service. Material changes to data access, execution authority, or business purpose deserve review. Routine administration can follow established procedures. If the original owner moves on, responsibility needs to transfer or the service needs a retirement decision. The inventory should preserve that connection between the owner, approved scope, and current review status.

Reviews should lead to action. If an incident exposes an ineffective control, the service owner should update it and verify the improvement. Successful experiments can also reveal capabilities worth offering more broadly. Carrying the same unresolved issue from one meeting to the next does not make the service more controlled.

## Give People a Way Forward

I want a team with a useful idea to know how to get started. Give them an approved place to work, explain the boundaries, and make sure somebody can answer when they reach a decision they cannot make themselves.

We will not anticipate every situation. That is part of introducing something new. What we can do is make decisions deliberately, assign responsibility, and revisit the arrangement when the work changes. If the process keeps producing confusion, we need to improve the process too.

The next chapter turns to spending. Before people start consuming a service, we need to understand the financial commitment and how we will control it. Finding out how an experiment went by opening the invoice is an expensive feedback mechanism.
