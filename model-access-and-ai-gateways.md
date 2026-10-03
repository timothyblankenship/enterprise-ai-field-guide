# Model Access and AI Gateways

Application owners do not usually thank us when Azure Policy blocks something they are trying to build. From their perspective, they have work to do, and the platform just told them no.

We block public IP creation by default. If a team needs an exception, it requires review by the CCoE, Network, and Security before an exemption is granted. The CCoE also helps review the architecture and guide the team toward an appropriate approach.

In many cases, the team discovers that a private IP works just fine for its use case. The initial request was for a particular configuration. The conversation helps us understand the actual requirement.

Those controls also helped us when audit time came around. We had enforced requirements during deployment instead of relying entirely on finding and correcting problems afterward. Application owners felt the restriction immediately. The benefit was less visible during their day-to-day work.

That experience shapes how I think about shared access to AI models. A team needs to understand what is available, why a restriction applies, and how to proceed when the approved options do not meet its needs. A denial should come with enough explanation to take the next step. Sometimes that means using the supported approach. Sometimes it means reviewing an exception.

An AI gateway can help put those decisions into operation. Its usefulness depends on how well it connects the controls we need with the work people are trying to do.

## Give Applications a Supported Way In

As more teams build AI capabilities, the same access questions begin appearing in different projects. Developers need to connect to a model, authenticate, select an approved option, and understand the limits on their use. The organization needs to know who owns that activity and how it will be supported.

Leaving every team to solve those questions independently creates repeated work. It can also produce a collection of provider accounts, credentials, and configurations that nobody sees as a whole.

A shared access service gives teams a supported starting point. An AI gateway is one possible component of that service. It sits between an application and model endpoints, receiving requests and forwarding them according to its configuration.

This chapter focuses on API access: applications and agents making programmatic requests to models. A company-built chat interface can use that gateway behind the scenes. Employees using a provider's hosted chat product generally operate through that product's own administrative controls. Purchasing a gateway does not automatically place those conversations under its control.

Depending on the implementation, a gateway may enforce access rules, route requests, apply usage limits, cache responses, and collect operational or consumption information. Some also translate between provider interfaces or integrate security screening. These capabilities vary, so the design needs to distinguish what the selected gateway supports from what we intend to build around it. [Microsoft's AI gateway capabilities](https://learn.microsoft.com/en-us/azure/api-management/genai-gateway-capabilities), [Cloudflare's AI gateway features](https://developers.cloudflare.com/ai-gateway/features/)

The service includes more than the gateway software. Someone still needs to define the approved offerings, fulfill access requests, support developers, and operate the connections to providers.

That is familiar platform work. The value comes from making it repeatable enough that the next application team can get started without assembling the entire arrangement again.

## Carry the Approval Into Provisioning

Consider a team building an internal application that summarizes service requests. It has an approved purpose, an accountable owner, and funding. The next step is giving the application access that reflects those decisions.

The request should establish which model capabilities it needs, the environment it will run in, and the information it is permitted to send. Provisioning should connect that application to its approved configuration and financial attribution.

The application should have an identifiable way to access the service. A shared credential copied between unrelated projects makes it harder to distinguish their activity, adjust their limits, or remove access to one without affecting the others.

Where supported, workload identities and short-lived credentials can reduce the need to distribute long-lived secrets. Where keys are required, they need secure storage, controlled access, and a lifecycle. Keeping provider credentials behind the gateway can simplify that responsibility for application teams, but the gateway's own credentials still need to be managed.

The developer should receive enough information to use the service successfully: how to connect, which models or capabilities are available, what limits apply, and where to get help.

I want that experience to feel like an approved service we know how to deliver. If the instructions end with finding someone who has a working key, we have more platform work to do.

## Apply the Shared Rules Consistently

One benefit of a shared gateway is having a place to enforce supported requirements across applications. We can configure access to approved models, apply workload limits, and preserve usage identifiers without asking every development team to recreate those controls.

That is part of what made our Azure Policy approach useful. We did not rely entirely on each application owner remembering the requirement during deployment. The platform enforced a default, and the exception process handled needs outside it.

The same principle applies to model access, although the enforcement mechanisms are different. An application's entitlement should determine which routes it can use. A request for a different model should not succeed simply because someone changes the model name in the application.

Central enforcement can reduce inconsistent implementations, but it also concentrates responsibility. A poorly scoped rule can affect several teams. The people operating the service need to understand which workloads a setting applies to and what happens when it takes effect.

These controls cover the traffic passing through the gateway. If applications can use separate provider credentials and connect directly, publishing a shared endpoint does not make its use mandatory. Identity, network controls, and provider access arrangements may also need to support the intended design.

We should be able to explain both the approved path and how we know applications are using it.

## Make Restrictions Understandable

A gateway may reject a request because the application is not authorized for the selected model, has exceeded a limit, or is attempting an unsupported operation. The response needs to help the team distinguish those conditions from a service failure.

This is where the public IP example carries over. A restriction without context looks like an obstacle. An explanation and a supported alternative give the team something to work with.

Suppose the service-request application asks for a model outside its approved offering. The developer needs to know which options are available and how to request a review if those options cannot perform the task adequately. Repeatedly submitting the same request should not be the only obvious next step.

An exception should involve the people responsible for the additional exposure. Depending on the request, that could include the service owner, AI CoE, security, data owners, or the person approving the expense. The gateway administrator should not have to invent that decision while troubleshooting a ticket.

Some exceptions will be justified. Others will reveal that the existing offering can satisfy the requirement with a different configuration. Both outcomes are useful when the conversation helps the team finish its work.

The approved path also needs to be practical. If it is unreliable, poorly documented, or takes weeks to obtain, teams will have reasons to seek another route. We should examine that experience with the same seriousness we apply to the restrictions.

## Keep Routing Connected to the Intended Use

A gateway can provide a common endpoint while directing requests to different model services. That can simplify provider connections for applications and give the platform team a place to apply routing rules.

Routing determines where a request goes. Load balancing distributes requests across deployments to share demand. Those deployments might serve the same model. More elaborate routing can select different models according to the task, but that introduces another decision about expected behavior.

A common interface does not make models interchangeable. Two services may accept similar requests while differing in supported inputs, tool interactions, output formats, or response quality. An application expecting a particular structured response still needs to handle and validate what it receives.

Routing should follow the evidence from model evaluation. If an application has been tested with one model, sending it to another requires enough validation to establish that the alternative can perform the assigned role.

Data-handling requirements also follow the request. Every permitted destination needs to remain within the approved arrangements for processing that information. Availability should not quietly override those boundaries.

For the service-request application, a supported route might be straightforward: requests from that workload go to the model approved for summarization. There is no need to introduce a complicated selection mechanism before the workload benefits from one.

## Treat Fallback as an Application Decision

Fallback provides an alternate path when the primary service cannot complete the request. That is different from distributing normal traffic across available deployments.

If the primary model fails, an application might use an evaluated alternative, queue the work, or ask the user to try again later. The appropriate response depends on the task and the consequences of receiving a different result.

For a drafting function, an alternative model may provide an acceptable degraded service. For a workflow that relies on a particular output structure or behavior, substitution may create more problems than a clear failure.

Retries deserve similar care. A request that times out may already have been processed, even if the response never reached the application. Repeated attempts can increase consumption. If the application then uses model output to trigger actions, it also needs protection against repeating those actions.

The gateway can participate in recovery behavior, but it does not know every business consequence downstream. The application and platform teams need to agree on what gets retried, what can be rerouted, and when the work stops.

The useful question is whether the alternate path preserves an acceptable service. Keeping the connection alive is only part of that answer.

## Cache Where Reuse Makes Sense

Some requests do not need a new model response every time. A gateway may be able to return a previously stored answer, reducing latency and avoiding another inference call.

Exact-match response caching looks for a matching request according to the configured cache rules. Semantic caching looks for a sufficiently similar request. That can broaden reuse, but similarity does not establish that two requests deserve the same answer. [Cloudflare's gateway features](https://developers.cloudflare.com/ai-gateway/features/), [Microsoft's semantic caching guidance](https://learn.microsoft.com/en-us/azure/api-management/azure-openai-enable-semantic-caching)

Consider two employees asking the same question about an internal process. If their permitted information differs, a response generated for one may be inappropriate for the other. The words in the question are only part of the context.

The cache design needs to account for the information and permissions that influence the answer. Relevant differences may include the user or access group, source material, instructions, and model configuration. The cache itself also becomes a place where potentially sensitive information is stored.

Freshness matters just as much. A response about a published policy may remain useful until that policy changes. A response about the current status of a service request may become outdated moments later. Expiration and invalidation need to reflect the work, and some requests should bypass caching entirely.

Provider-side prompt caching is a different mechanism. It reuses processing associated with repeated input rather than returning a previously generated answer. The model still generates a response, and the provider's support, eligibility rules, and pricing determine the benefit.

I would rather repeat a model call than return another person's information or a stale answer that sends someone down the wrong path. The savings are useful when the reuse is appropriate.

We will examine the financial impact in the FinOps chapter. Here, the architectural decision is whether a result can safely be reused and what must be true before that happens.

## Connect Consumption to the Workload

A shared access layer can help connect model usage to the application responsible for it. That gives platform teams and FinOps a useful place to understand demand and investigate unexpected consumption.

The records need to preserve meaningful identifiers. An application, environment, and responsible cost center are more useful than a large total attributed to a single shared provider account.

Limits should also reflect the workload. A development experiment may need a different allocation from an established production application. Request rates, concurrency, and usage quotas can help control demand, although the available mechanisms depend on the gateway and provider.

As we discussed in Chapter 5, a usage limit is not automatically a precise financial ceiling. Models can have different prices, requests can vary in size, and some costs sit outside the gateway entirely.

Caching adds another distinction to reporting. We should be able to separate requests served from a cache from requests sent to a provider. A cache hit may avoid a model call while still incurring gateway, cache, or supporting infrastructure costs.

An application may also incur charges for retrieval, storage, tools, and other services. Gateway reporting can explain part of the bill without representing the full cost of delivering the application.

The financial relationship established during approval needs to survive this technical path. If the gateway can tell us which application made a request but we cannot connect that application to an owner, we have stopped one step short of useful attribution.

## Understand What the Gateway Cannot See

Even within its traffic, the gateway has limited context. It may know which application is calling and which model is requested without knowing whether a particular employee is entitled to every document included in the prompt.

That permission needs to be enforced where the application retrieves and assembles the information. Once restricted content has been supplied to the model, an instruction to keep it secret is a weak substitute for controlling access in the first place.

Some gateways offer content screening or integrate data-protection and prompt-attack detection. Those can contribute to a layered design, but their presence does not establish that every sensitive input or harmful instruction will be detected.

The same boundary applies to tools. A gateway handling model calls may not sit between the application and the systems where actions are executed. Those systems still need authorization, input validation, and controls over consequential operations.

A hosted chat service or an AI feature embedded in another business application may operate entirely outside this architecture. Its access, data handling, and administration still need attention through the controls available for that service.

I want us to be able to explain where each requirement is enforced. "The gateway handles security" is too broad to help the engineer implementing the application or the person investigating an incident.

## Operate the Service People Depend On

Once applications depend on a shared access service, its operation becomes part of their reliability. An outage or incorrect configuration can affect several teams at once.

The service needs a named operating owner, support arrangements, and visibility into its own health and provider dependencies. Teams should understand how they will learn about an interruption and where responsibility moves when a problem originates outside the gateway.

Useful telemetry might include request volume, latency, errors, rejected requests, cache behavior, and the route used. That information can help distinguish a provider problem from an application exceeding its allocation or requesting an unsupported capability.

Logging requires judgment. Prompts and responses can contain company information, so collecting everything by default may create another sensitive data store. Record what is needed for operation and investigation, with appropriate access and retention controls.

Capacity and failure behavior need attention too. A shared service should not become the place where one application's demand quietly consumes the capacity everyone else expects to use. Workload isolation and appropriate limits help make that behavior more predictable.

We will cover reliability and incident response in later chapters. At this stage, the architecture needs to recognize that a shared gateway is a service with dependencies and failure modes of its own.

## Make the Supported Path Worth Using

The lesson from our public IP restrictions was broader than the setting itself. We had a default, people responsible for reviewing exceptions, and a team that could help application owners work through the architecture. The control and the assistance belonged together.

I would apply that same approach to model access. Give developers an approved way to connect, make the available capabilities clear, and preserve the ownership and financial decisions behind the request. When the offering falls short, provide a review path that can produce a useful answer.

A gateway can help us deliver that experience consistently. Its success depends on whether the surrounding service makes it easier to build within boundaries the organization can support.

The next chapter looks behind those connections at inference, GPUs, and model serving: the resources and operating decisions involved in producing a model response.
