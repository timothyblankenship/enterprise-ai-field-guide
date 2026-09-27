# Anatomy of an Enterprise AI System

An employee opens an internal AI application and asks which production applications will be affected by database maintenance this weekend. A few seconds later, the application returns a list, explains the dependencies, and links to supporting records.

From the employee’s perspective, that is one interaction. Behind it, several systems had to cooperate. The application needed to identify the employee, find the maintenance records, inspect application dependencies, enforce access permissions, and assemble enough information for a model to produce a useful answer. Someone also needs to know whether the information was current and what happened if part of that process failed.

This is where the technology stack becomes a working system. Understanding the components individually helps, but following a request through them reveals the dependencies that deserve attention.

I have worked on enough infrastructure migrations to pay attention to those connections. Moving a server is one task. Understanding what still connects to it, which identity those connections use, and what happens when an address changes can take considerably more effort. An AI application introduces some unfamiliar components, but it gives us plenty of familiar opportunities to overlook a dependency.

There is no universal enterprise AI architecture. A forecasting model that runs overnight does not need the same design as an interactive assistant. A document-processing workflow differs from an agent authorized to change infrastructure. We can, however, examine the recurring components and understand the responsibilities they carry.

## Start With the Request

Consider the maintenance question more closely. “Which applications are affected?” sounds straightforward because the employee already understands the business context. The system has to locate and connect the evidence.

The maintenance schedule might be in a change-management system. Database ownership could be in a configuration database. Application dependencies might come from monitoring, discovery tools, or documentation maintained by individual teams. Some of that information may disagree.

The application also needs to respect who is asking. An employee may be allowed to view one environment but not another. Some maintenance records may contain sensitive operational details. The service identity used to query those records could have broader access than the employee, which makes authorization a responsibility throughout the request.

Once the system gathers the information, it must decide what to provide to the model. That may include the question, relevant records, instructions about how to handle missing evidence, and source identifiers for citations. The model produces a response, which the application may validate or format before presenting it.

Meanwhile, operational systems record enough evidence to investigate the interaction. That could include which services were called, how long they took, which model handled the request, and whether retrieval or tool execution failed. Sensitive content requires appropriate access and retention controls.

A useful answer depends on the whole sequence. If the dependency data is incomplete, the model cannot reliably fill in the missing relationships by sounding confident. The application needs a way to communicate that limitation.

## The Application Layer

Users usually interact with an application rather than a model directly. That application might be a chat interface, a document-processing service, a coding tool, or an AI capability inside software they already use.

The application translates a business interaction into requests the system can process. It collects input, manages state, invokes supporting services, and determines how results appear to the user. It may display citations, request approval, or combine model output with conventional application logic.

Those choices influence how much trust a user places in the result. An answer presented as a confirmed operational fact creates a different expectation from one that identifies missing records and asks for review. If the application hides uncertainty that the underlying system detected, the interface has introduced a problem of its own.

AI applications still need ordinary software engineering. Authentication, input validation, session management, error handling, and dependency management remain necessary. A model response also needs to be treated as output requiring appropriate validation, especially if another part of the application will interpret it as an instruction or use it to perform an action.

This is familiar territory from application development. A useful component does not relieve the application of responsibility for how its output is used. We need to design the experience around what the system can establish, including the cases where it cannot complete the work.

## Identity and Authorization

Identity is often shown around the outside of an architecture diagram as a security concern. In practice, it participates in the request from the beginning.

The employee’s identity may influence which data can be retrieved, which models are available, and whether a requested action is permitted. Applications and supporting services also need identities to communicate with one another. An agent invoking a tool may operate under its own service identity or some form of delegated user authority.

Those choices need to be deliberate. If a retrieval service can read an entire repository, the application still has to enforce the requesting user’s access restrictions. Broad access granted for ingestion must not quietly become broad access for everyone asking questions.

Actions make the distinction especially important. An employee asking an agent to restart a server has not automatically established that the employee or the agent is authorized to do so. The execution service needs to check the operation, target, and relevant authority before acting.

Enterprise systems already have approaches for workload identities, delegation, least privilege, and separation of duties. AI creates additional places to apply them. The enforcement should live in the services and tools that control access, where it can be tested and audited.

A prompt telling an agent to respect permissions is useful guidance. The infrastructure still needs to prevent an unauthorized operation.

## Model Access

An application can call a model endpoint directly. For a limited workload, that may be entirely reasonable. As the number of applications and providers grows, managing each connection separately can become difficult.

A model-access layer or AI gateway can provide shared capabilities such as authentication, approved model catalogs, routing, quotas, rate limits, and usage attribution. It can also give application teams a consistent way to request a model without embedding every provider-specific detail into their applications.

That fits the same platform principle I apply to routine infrastructure. Teams should have a usable path to an approved capability, with common requirements built into the service. They should not have to recreate access controls and cost reporting for every application.

Routing can account for workload requirements. A constrained task might use a smaller model that has demonstrated adequate quality. Another request might require a larger context window or a model approved for particular data. Capacity and cost can also influence the decision.

The abstraction has limits. Models can behave differently even when their interfaces look similar. Switching providers or model versions still requires evaluation of the application’s behavior. A common endpoint makes integration easier; it does not establish that every model behind it is interchangeable.

A gateway also governs only the traffic that passes through it. An AI feature inside a SaaS application may use a completely separate path. The enterprise needs to understand both rather than assume one shared service controls the entire AI footprint.

## Inference and Model Serving

Inference is the process of executing a trained model on an input to produce an output. For an application using a managed endpoint, much of the infrastructure involved is hidden behind the API.

The serving system still has to load model weights, allocate memory, schedule requests, and manage capacity. Language-model serving may involve batching requests and maintaining caches that reduce repeated computation. Those mechanisms influence latency, throughput, and cost.

Interactive applications care about how quickly a response begins and how quickly it completes. A background processing job may place more emphasis on overall throughput. Optimizing for one does not automatically optimize for the other.

Self-hosting makes those tradeoffs more visible. Model size, numerical precision, concurrency, and context length all affect resource requirements. Quantization can reduce memory use, but its effect on quality and performance needs to be evaluated for the deployment. Larger models may require multiple accelerators and additional coordination between them.

Infrastructure teams will recognize the broader problem: match resources to the workload and the service expectations. AI serving introduces different resource behavior, but capacity planning remains necessary.

We also need to account for variation in demand. A service that performs well during a small pilot may behave differently when a large group begins using it at once. The capacity plan should describe what happens at that limit, including queuing, throttling, or a clear failure response.

## The Models Inside the Application

A box labeled “Model” can hide several distinct responsibilities. An enterprise application might use an embedding model to represent documents for search, a reranking model to improve result ordering, and a language model to generate the response.

Other applications may use forecasting, vision, speech, or specialized classification models. The architecture should identify their roles instead of treating every AI component as the same kind of service.

Each model introduces lifecycle decisions. The enterprise needs to know which version is deployed, where it came from, how it was evaluated, and which applications depend on it. A change to an embedding model, for example, may require changes to the corresponding retrieval index. Updating one component independently can affect the behavior of another.

This is where inventory becomes operationally useful. A list of model names is a starting point, but the dependencies matter. When a model is retired or replaced, someone needs to understand the affected workloads and the evidence required before they can move.

Replaceability is valuable when it is supported by evaluation and controlled deployment. Otherwise, “we can switch models” may turn out to mean that the connection works while the application behaves differently in ways nobody checked.

## Data and Context

A model receives information through the input supplied to it. In an enterprise application, that input may include instructions, the user’s request, retrieved documents, tool results, conversation history, and application state.

Together, these form the context for the interaction. Constructing that context is an engineering responsibility because the system has to select what is relevant, respect access restrictions, and work within the model’s available capacity.

Sending everything is rarely a sound approach. Irrelevant information consumes capacity, increases processing cost, and can make it harder for the model to use the evidence that matters. Sensitive information can create an exposure even when it contributes nothing to the answer.

Freshness also matters. In the maintenance example, a useful architecture might retrieve explanatory documentation while querying current maintenance and dependency records directly. Different sources can require different access patterns within the same request.

Information supplied as context also needs to be treated according to its trust level. A document or tool response may contain text that attempts to redirect the model’s behavior. The application should not allow content retrieved as evidence to acquire authority over the system’s instructions or permissions.

Context construction therefore involves more than assembling a large prompt. It includes source selection, access control, relevance, freshness, and boundaries around how external content can influence the system.

## Retrieval Is a System of Its Own

Retrieval often begins well before the employee asks a question. Content has to be collected from source systems, parsed, and prepared for search. Documents may be divided into smaller sections, metadata preserved, and embeddings generated. Search indexes then need to remain aligned with the underlying sources.

At request time, the system may use keyword search, semantic search, or a combination. Results can be filtered and reranked before selected information is passed to the model.

The ongoing maintenance can be as important as the initial build. A source document changes. An employee loses access. An application is retired. A record is deleted. The retrieval system needs a reliable way to reflect those events.

Otherwise, the enterprise creates a second representation of its information that becomes less trustworthy over time. It may still return convincing answers because the model has no automatic way to know that the source should have been removed.

Authorization must remain effective as the data moves through ingestion, indexing, retrieval, and presentation. The implementation can vary, but the result must preserve the access restrictions that apply to the person using the application.

Source traceability helps both users and support teams. A citation allows someone to inspect supporting material. Operational evidence should also make it possible to identify which version or retrieved content contributed to the answer, subject to the system’s data-handling requirements.

The objective is a retrieval service that remains useful as the enterprise changes, rather than one that works only on the document collection used in the demonstration.

## Tools and Execution

Tools give an application or agent access to capabilities outside the model. A tool might query a database, inspect monitoring data, create a ticket, or invoke an infrastructure API.

The tool interface should describe the operation clearly, but its implementation must also enforce the appropriate controls. Inputs need validation, permissions need checking, and results need to communicate enough information for the caller to distinguish success from failure.

For an action, the target and scope are especially important. A restart operation should identify the actual resource and environment involved. An approval should correspond to the operation that will be performed. If the parameters change, the system may need another approval.

Tools also need useful failure behavior. A timeout can mean the action failed, or it can mean the caller did not receive confirmation of an action that succeeded. Where possible, execution interfaces should support safe retries or a way to check the status of a prior request.

These details are easy to underestimate because the model-facing interface may be small. The capability behind it can be significant. A few lines describing a tool can expose an operation with consequences across a production environment.

We should evaluate tools according to what they can affect and the authority they carry. The convenience of invoking them through natural language does not reduce that responsibility.

## Agents and State

An agent adds execution over multiple steps. It can use model outputs to select tools, inspect results, and decide what to do next within the limits of its design.

That creates state that a single request may not have. The system needs to track what has been attempted, what has completed, and what remains unresolved. It may also need to pause for approval or resume after an interruption.

Limits are part of the design. An agent may need a maximum number of steps, a time budget, consumption limits, and conditions that cause it to stop or escalate. Without them, a task can keep consuming resources while making little progress.

Observability needs to follow the execution. Recording the final answer is insufficient when the agent made several model calls and attempted actions along the way. The support team needs evidence of the context supplied, tools invoked, results returned, and approvals obtained.

That evidence should describe what the system actually did. We do not need to assume that a model’s explanation of its reasoning is a complete or reliable account of the execution. Tool records, state changes, and service responses provide more concrete ground for investigation.

A useful execution record lets an engineer determine where the process diverged from expectations and whether recovery can proceed safely. Discovering during an incident that we saved only “Sorry, something went wrong” is a lousy introduction to the application.

## The Systems That Make It Operable

Several responsibilities span the entire request path. Security covers identities, data, interfaces, and actions. Observability provides evidence about execution and service health. Evaluation helps determine whether the application is producing acceptable outcomes.

Reliability engineering addresses dependencies, failure handling, and recovery. FinOps connects resource consumption to the workload and its value. Governance establishes requirements and ownership, while risk and compliance functions define additional obligations and evidence where applicable.

These responsibilities need to appear in the implementation. If usage cannot be attributed to an application or team, cost accountability becomes difficult. If the deployment process does not record which model and configuration were released, investigating a quality change becomes harder. If access controls are left to a future phase, the pilot may become a dependency before that phase arrives.

Recovery also depends on understanding the components together. Restoring application code may not restore its retrieval index, access configuration, or agent state. Some components can be rebuilt, while others require preserved state. The recovery design needs to account for the time and dependencies involved.

For applications using managed services, provider recovery does not automatically recover the whole business process. The enterprise still needs to understand its responsibilities and how users will continue essential work during a disruption.

These are operational requirements to establish while designing the system. They become more expensive to reconstruct after users depend on it.

## Follow the Connections

Return to the employee asking about weekend maintenance. A useful answer requires current records, meaningful dependency information, appropriate access, and a model capable of working with the supplied evidence. It also requires an application that communicates limitations and an operating team that can investigate failures.

The model contributes to the result, but the relationships between components determine much of what the system can reliably deliver. A retrieval service with stale permissions, an overprivileged tool, or an untested recovery dependency can undermine an otherwise capable application.

That is the practical value of understanding the anatomy. We can trace a request, identify where information and authority cross boundaries, and assign responsibility for what happens there. We can also recognize when a simpler design would satisfy the task with fewer dependencies to support.

The next step is to make those choices deliberately. Knowing which components are available gives us options. Designing the architecture means selecting and connecting them around the work the business needs to accomplish.
