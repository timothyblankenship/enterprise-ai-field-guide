# Enterprise AI Is a Technology Stack

I have spent much of my career helping people introduce new technology while keeping the systems people already depend on running reliably. That has included servers and desktops, virtualization, cloud migrations, platform engineering, and the teams responsible for keeping those things running. The technology changes. Someone still needs to deploy it, secure it, support it, and explain the bill.

I like building things, especially when the result makes someone’s job easier. Early in my career as an application packager, I built an application that took information entered into a form and produced an installation package. I originally used ASP and SQL. Corporate told me that was not the approved technology stack, so I rebuilt it using ColdFusion and Oracle. The objective stayed the same: take a process that required manual effort and make it repeatable.

That interest followed me into cloud engineering and leadership. I believe developers should be able to get routine infrastructure through self-service automation instead of waiting for someone to build it by hand. Getting useful code into production sooner gives the business an opportunity to benefit sooner. Sometimes that means revenue. Sometimes it means a better customer experience or fewer hours spent doing something nobody enjoyed in the first place.

AI creates more opportunities to do that work. It also makes it easy to build a convincing demonstration before understanding what it will take to operate the result.

I encourage experimentation. People need room to try things, learn, and occasionally break something within an appropriate environment. Once others depend on what we have built, the responsibilities expand. We need ownership, access controls, visibility into failures, recovery procedures, and a reasonable understanding of cost. A successful demonstration does not complete those jobs.

That is the starting point for this field guide. Enterprise AI is a technology stack. The model matters, but its usefulness depends on the systems, information, controls, and people around it.

## We Already Had AI

Enterprises were using artificial intelligence long before employees could open a browser and ask it to write an email. Machine learning models predicted demand, detected fraud, classified images, recommended products, and identified equipment that might fail. Those systems often operated behind the scenes. The person using the result might never have known a model was involved.

Generative AI made the interaction more visible. People could ask questions in ordinary language, follow up, and work directly with the responses. Developers could begin with a broadly capable model instead of training a specialized model for every task. That made AI accessible to a wider group of users and builders.

The earlier forms of AI still have useful jobs to do. A manufacturing process might use a predictive model to identify a potential equipment failure, computer vision to inspect a component, and a language model to help a technician interpret maintenance documentation. An application could use those results to prepare a work order. Several forms of AI can contribute to the same business outcome.

An enterprise AI strategy therefore needs to account for more than conversational applications. Language models deserve attention, but so do the other models already supporting business processes.

Model selection also needs to fit the work. A smaller model may perform adequately on a constrained classification or extraction task, with different cost and performance characteristics from a larger one. We need to evaluate that against the actual workload. Infrastructure teams have plenty of experience buying more capacity than a task needs. We do not have to preserve every tradition.

## The Model Is Part of the System

Consider an internal application that answers questions about company information. An employee enters a question and receives a response. Behind that interaction, the application establishes the employee’s identity, locates relevant information, checks access, constructs a request for a model, and presents the result.

Each step influences the outcome. A capable model can accurately summarize the wrong document. It can also summarize a restricted document for someone who should never have received its contents. An application can retrieve an outdated procedure and return an answer that sounds reasonable until someone follows it.

Model selection alone cannot resolve those problems. Identity establishes who is making the request, while authorization determines what that person can access. Retrieval and context construction influence the information available to the model. Application design shapes how the result is presented and whether the user can inspect its sources. Telemetry gives the support team evidence when something goes wrong.

Infrastructure and service design affect whether the application performs reliably. Recovery arrangements determine how it returns after a disruption. Cost influences whether the enterprise can afford to provide the capability at the scale people want to use it.

My experience spans application development and the infrastructure underneath it. That is one reason “stack” feels like an appropriate description. Users experience the application as a whole, regardless of how many products, providers, and teams contribute to it. When an employee receives an incorrect answer, explaining that the model endpoint was healthy does not get us very far. We need to understand the path that produced the result.

The layers are not always arranged neatly. A managed service may provide several capabilities, and an application may depend on multiple providers. The value of thinking in terms of a stack is that it makes those dependencies and responsibilities visible.

## AI Enters Through Several Doors

The enterprise does not acquire AI through a single process. Some capabilities arrive through deliberate engineering projects. Others appear inside products the organization already owns.

A SaaS provider may add an assistant to an existing application. A developer may connect an internal service to a model API. A security team may enable an investigation feature in its monitoring platform. A business unit may purchase a specialized AI product, while an engineer experiments with an open-weight model in a separate environment.

These approaches give the enterprise different responsibilities. With SaaS, the provider operates much of the underlying system, while the customer still makes decisions about identity, configuration, data handling, usage, and cost. Building an application internally gives the enterprise more control and more engineering work. Hosting the model adds responsibility for serving infrastructure and capacity.

This complicates discovery. An application inventory can remain almost unchanged while the capabilities inside those applications change significantly. A process that records only newly approved AI projects will miss features enabled in existing products and experiments that have quietly become part of someone’s working day.

I understand why people start building when they have a problem and an accessible tool. That initiative is valuable. We should provide a practical place to experiment, with clear limits on the data, systems, and spending involved.

The approved approach also needs to be useful. If developers can obtain appropriate model access, understand the requirements, and follow a reasonable deployment process, we give them a reason to work within it. Making routine work unnecessarily difficult encourages people to find another route. Publishing a policy does not change that behavior by itself.

## The Pace Has to Fit the Business

I have worked in software companies that adopted new technology quickly and made decisions while the situation was still developing. I now work in manufacturing, where the pace can be more deliberate. There are good reasons for that difference.

A change affecting a production process may need validation against equipment dependencies, product quality, safety requirements, and a narrow maintenance window. The availability of a new capability does not mean the business should immediately put it into service. Manufacturing also contains opportunities for experimentation that do not require putting production at risk.

Software businesses have workloads where failure carries serious consequences, too. The industry label does not settle the decision. We need to understand the particular system, who depends on it, and how easily a mistake can be contained or reversed.

That distinction matters for AI adoption. An assistant drafting an internal document and an agent changing an operational system should not inherit the same deployment expectations simply because both use a language model. Teams can move quickly on an isolated experiment while applying more deliberate controls to a capability that affects live operations.

My support for faster delivery includes that judgment. We should remove avoidable delays and automate repeatable work. We should also take the time required to establish that a consequential change is ready. Speed is useful when it helps the business accomplish something without creating an unacceptable problem elsewhere.

## Enterprise Data Makes the Difference

A model may explain how a database works without knowing anything about the database supporting your order-processing application. To help investigate why that application failed last night, it needs evidence from your environment.

That evidence may be spread across monitoring telemetry, logs, incident tickets, change records, configuration data, source code, and documentation. Finding the relevant information and making it available in a useful form becomes a substantial part of the engineering.

Retrieval-augmented generation, usually called RAG, is one approach. The application retrieves information and supplies it as context when invoking the model. This allows the response to draw on enterprise information that was not part of the model’s training.

The basic idea is straightforward. Maintaining a useful implementation takes more work. Documents change, sources disappear, indexes need updating, and ownership may be unclear. The system needs to preserve enough information about its sources to support citations and investigation. It also needs to handle evidence that is incomplete or contradictory.

Access control is essential. A retrieval service may have access to a document that the person asking the question is not authorized to read. If the system passes that document to the model and returns its contents in a summary, it has exposed the information even without displaying the original file.

Retrieval therefore belongs in the enterprise data-access architecture. Authorization, classification, freshness, and source ownership need to be part of the design. Connecting a model to a collection of documents does not resolve those responsibilities.

Context extends beyond documents. It can include conversation history, application state, instructions, user information, and results returned by tools. The information we include, omit, or allow to become stale influences the output. Prompt design is useful, but it is only one part of engineering that information environment.

## Agents Bring Actions Into the Picture

An application that recommends a change and one that executes it require different controls. They may use the same model, but they create different operational consequences.

Suppose an assistant reviews an infrastructure alert and recommends restarting a service. An engineer can inspect the recommendation, consider the dependencies, and decide whether to proceed. If an agent can invoke the infrastructure API itself, some of that decision process and execution authority has moved into software.

For this field guide, an agent is a system that can select steps and use tools toward a goal, with varying degrees of freedom. Some operate within tightly constrained workflows. Others have more discretion. We will examine those differences because the label alone tells us little about what a system can do.

The authority we grant is immediately important. An agent needs an identity, defined permissions, limits on the resources it can affect, and a record of attempted actions. Some operations may require human approval. The surrounding system must enforce those restrictions rather than relying entirely on instructions supplied to the model.

We have spent years dealing with overprivileged service accounts. Giving one a conversational interface does not improve the permissions.

Execution also introduces recovery problems. If an action succeeds but its confirmation is lost, retrying may repeat the action. If an agent completes only part of a workflow, someone needs to understand what changed before continuing. A final message saying the task failed does not provide enough information to make that decision.

These concerns should shape how we build useful automation. I want systems that remove repetitive work and help people deliver. I also want the team responsible for the service to understand what the automation can affect, how to observe it, and how to stop or recover it when necessary.

## Someone Still Runs the Infrastructure

Across virtualization and cloud migrations, we have repeatedly changed how infrastructure is provided and who operates it. AI introduces another set of workloads into that history.

A managed model endpoint provides a convenient abstraction. The enterprise sends requests without operating the underlying serving infrastructure. Capacity and performance still appear through latency, throughput, quotas, availability, and pricing. The abstraction reduces some responsibilities while leaving the application dependent on a service it does not directly control.

Hosting a model brings more of the infrastructure into view. Model size and numerical precision affect memory requirements. Concurrency, batching, and serving software influence performance. Some deployments require GPUs or other accelerators, while smaller workloads may have other options. The architecture affects both user experience and resource efficiency.

An expensive accelerator sitting mostly idle still costs money. Spare capacity may nevertheless be justified by latency requirements, demand spikes, or resilience needs. Utilization is useful evidence, but it needs to be interpreted in the context of the service.

The Cloud Center of Excellence and FinOps are under my leadership, so these decisions connect directly to responsibilities I carry. Teams need useful platforms, the business needs reliable services, and someone needs to understand whether spending supports the intended outcome.

Negotiating a contract is part of that work. Understanding consumption after the contract is signed is another. A favorable price per unit can still produce an expensive application if it makes unnecessary model calls, supplies excessive context, or repeatedly retries unsuccessful work. The cost of completing the business task is often more informative than the price of an individual request.

We will examine model serving and infrastructure economics later in the book. Even when a provider operates the hardware, understanding those foundations helps explain the service behavior and the bill.

## Operations Need More Than a Green Dashboard

During an MSP transition, I brought alerting into our own tools to prevent gaps as we moved away from the provider. Moving the services was only part of the responsibility. We also needed visibility as ownership changed so that we could identify problems and respond to them.

An AI application can create a similar gap. A team may take responsibility for it without enough visibility into model calls, retrieval services, or tools. The application appears available, but the people supporting it cannot explain why its results have deteriorated.

Conventional health signals remain necessary. Availability, errors, and response times still matter. They do not tell us everything about quality. A request can complete successfully and contain a poor answer. Retrieval can return irrelevant information without producing a technical error. A tool can execute successfully even when the action was inappropriate.

Operations therefore need evidence about both execution and outcomes. Depending on the application, this may involve evaluation against representative tasks, inspection of retrieval results, records of tool activity, and feedback from users. Capturing that evidence requires care because prompts, context, and responses may contain sensitive information. Logging everything without considering access and retention creates another problem to support.

The team also needs a clear operating model. Responsibilities should cover investigation, escalation, changes, and decisions to limit or withdraw a capability. Component owners remain important, but someone must be accountable for the overall service. The employee asking for help should not have to navigate our organizational chart.

## Recovery Is Part of the Stack

Monitoring helps us recognize a failure. Resilience measures help the service withstand certain failures. Disaster recovery addresses how we restore service after a disruption that exceeds those protections. These responsibilities belong in the architecture from the beginning.

An AI application can depend on more recoverable state than its interface suggests. Application code, configuration, model versions, prompts, access policies, source data, retrieval indexes, and workflow state may all affect whether the restored service behaves as intended. The requirements depend on how the system is built and which responsibilities sit with providers.

Some information can be rebuilt from authoritative sources. A retrieval index, for example, might be regenerated rather than restored from backup. That is only a useful recovery approach if the sources remain available and rebuilding can finish within the required time.

An alternate model endpoint needs more scrutiny than a connection test. It may behave differently, support different features, or have different approval for handling the application’s data. A fallback should be evaluated before an outage makes it urgent.

Agents add another complication because a disrupted workflow may already have changed something. Recovery needs to distinguish completed actions from pending ones so that restarting the process does not repeat work with unintended consequences.

Business requirements should establish how quickly the service needs to return and how much data or state can be lost. Recovery exercises then provide evidence that the design can meet those expectations. An endpoint responding again is a useful milestone. We also need to verify that the restored application has the appropriate information, permissions, and behavior.

Business continuity extends that thinking to the people and processes that depend on the application. They may need a manual procedure, an alternate service, or a way to continue essential work while recovery happens. We will cover these subjects in more depth later, but they belong in our understanding of the stack now.

## Experimentation Needs Follow-Through

Moving from engineering into leadership challenged me, particularly the people side. Knowing the technology did not automatically mean I knew how to help a team work through uncertainty, disagreement, or failure. I had to develop those skills.

One principle I believe in is making it safe to report a problem. If people expect anger every time something breaks, we make it harder to learn what is happening. That is especially unhelpful when we are asking them to experiment with unfamiliar technology.

I believe in blameless postmortems and expect follow-through. We should understand the decisions people made, the information they had, and the conditions that contributed to the failure. The resulting work needs to produce a meaningful improvement.

“We’ll be more careful” does not give me much confidence. A permissions change, an automated check, or a tested recovery procedure gives us something to inspect. We cannot guarantee that nothing will fail again, but we can demonstrate that we addressed the failure we just experienced.

If the same mistake happens because the agreed work was never completed, that is where I get disappointed. Learning requires an owner and time to act. Otherwise, the postmortem becomes another document everyone meant to come back to.

AI experimentation should follow that principle. Give teams room to explore within appropriate boundaries, review the results honestly, and incorporate what they learn. A failed experiment can be valuable when it improves the next decision.

## Building a Stack People Can Use

The stack becomes visible once we consider these responsibilities together. Compute supports model execution. Access services connect applications to models. Data and retrieval provide context. Tools allow interaction with other systems, and applications make the resulting capabilities available to people.

Identity, security, evaluation, observability, recovery, and cost management span those components. Governance establishes requirements and accountability, while engineering turns those requirements into behavior the system can enforce.

No single product has to provide everything. The architecture needs to explain how the capabilities fit together, where responsibilities change, and what happens when a dependency fails.

It also needs to give teams a practical way to deliver. Self-service access, infrastructure as code, repeatable deployments, and appropriate evaluation can reduce delays while incorporating controls into the work. A useful platform makes routine tasks easier and helps teams recognize when an application needs additional attention.

That reflects why I like building in the first place. I want people to solve problems and get useful capabilities into service. I also want the people responsible for those capabilities to have the tools, information, and recovery options they need.

The rest of this field guide examines the stack in that spirit. We will work through the technology and the operational decisions that make it dependable, starting with what happens inside an enterprise AI system when someone actually uses it.
