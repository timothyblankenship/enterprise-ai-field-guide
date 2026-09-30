# Securing AI Applications and Data

If we put something into production, somebody needs to know how to stop it. I do not want us figuring that out while it is making changes we did not intend.

My first concern when something goes wrong is whether it is still causing damage. We may need to disable an integration, stop an automation, or revoke access before we have a complete explanation. Troubleshooting matters, but so does preventing the next bad action while we investigate the first one.

That does not mean shutting everything off without understanding the consequences. A service may support other applications, business processes, or people who depend on it. Stopping it can create a different problem, and potentially a bigger one. We need to evaluate that risk too.

Sometimes containment is the right decision. We might suspend an assistant's ability to make changes while keeping its read-only functions available. We might disconnect one tool or isolate an affected workflow while the rest of the service continues. Those options need to be designed and understood before an incident. Otherwise, the only control we have may be a very large off switch.

For me, confidence in an AI system includes knowing how we regain control when its behavior is wrong or unexpected. A successful demonstration tells us something about what the system can do. It tells us much less about whether we can interrupt it safely once it starts.

## Stopping the Assistant May Not Stop the Work

An AI system can hand work to other systems. It might submit a deployment, queue a job, or send a request to a business application. Stopping the assistant afterward does not necessarily cancel what it already set in motion.

That changes what we need to understand about containment. We need visibility into the work already handed off, a way to prevent further actions, and a clear understanding of which operations can still be stopped or reversed. Disabling the chat window is not much comfort if the deployment is still running.

The same applies to information. Cutting off access can prevent further retrieval, but it does not pull back data that has already been sent somewhere else. The response has to account for what happened before the control took effect.

These are design decisions as much as incident decisions. Before connecting an assistant to a consequential process, I want the team to explain how we would contain it, what would continue running, and what the business would lose while that restriction was in place. That conversation belongs alongside the demonstration of how well it works.

## When the Test Environment Reaches the Real World

In September 2026, Anthropic published an assessment of four incidents in which Claude models accessed real third-party systems without authorization during cybersecurity evaluations. According to the report, the models were told they were operating in simulations without internet access, but a configuration error left them connected to the internet. The evaluations also ran without the cybersecurity safeguards included in released products. These conditions matter when interpreting the incidents. [Anthropic's incident assessment](https://www.anthropic.com/research/alignment-assessment-cybersecurity-incidents)

The infrastructure part gets my attention immediately. We can tell a system that it is isolated, but the network configuration determines what it can actually reach. An instruction describing a boundary is not the boundary.

The report also identified model behavior that contributed to the incidents, including continuing harmful actions in pursuit of assigned tasks. My takeaway is that model behavior and infrastructure controls both need scrutiny. A diagram with a box labeled "sandbox" is a good start. I would prefer that the box also exist outside the diagram.

## Models Can Test Other Models' Defenses

There is real research behind the idea of models attacking models. In automated red teaming, researchers use one model to generate attempts to make another model violate its safeguards. The June 2025 Jailbreak-R1 paper, for example, describes training a model to produce varied and effective jailbreak prompts. This is a deliberately constructed testing process, not evidence that models spontaneously decided to fight each other. [Jailbreak-R1 research paper](https://arxiv.org/abs/2506.00782)

Defenses are improving too. Anthropic's January 2026 report on its next-generation Constitutional Classifiers described stronger resistance to jailbreaks while also reporting a remaining high-risk vulnerability found during testing. Better results deserve attention, but they do not establish that every future attempt will fail. [Anthropic's classifier research](https://www.anthropic.com/research/next-generation-constitutional-classifiers)

For an enterprise team, the useful question is what happens if a model does produce something it should not. A model refusing a harmful request is one layer of protection. The application's control over data, tools, and execution has to provide additional layers.

I want us to benefit from improvements in model safety without making the entire environment depend on the model always making the right call.

## The Instructions May Arrive Inside the Work

An assistant may encounter an attack while doing exactly what we asked it to do. A document, email, web page, or tool response can contain instructions intended to redirect its behavior. This is commonly called indirect prompt injection: content the assistant should treat as information attempts to become an instruction. [OWASP's explanation of prompt injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)

Imagine an assistant reviewing a supplier document. Hidden among the material is a direction to send internal information to an external destination. The employee's request was legitimate. The document introduced the hostile instruction.

The application should treat that document as material to analyze, not as authority to expand the task. We can reinforce that distinction through model instructions and screening, but we also need controls over where information can be sent and which actions the connected tools will accept.

This is where the permissions from the previous chapter become practical security boundaries. If reviewing a document does not require sending messages or changing records, those capabilities should not be available to that workflow.

## Follow the Information Beyond the Prompt

Protecting company information takes more than reminding employees not to paste sensitive content into a chat window. Information can also enter through connected repositories, retrieved documents, attachments, and tool results.

I want the team to follow that information through the service. The model provider may be one destination, but conversation storage, diagnostic logs, support systems, and downstream integrations can become additional copies. We need to understand retention and access across those locations.

Sending less information is often a useful starting point. If a task requires a few relevant fields, we should question why the application supplies an entire record. Credentials should stay in the systems that manage tool access rather than becoming part of the model's working context.

The answer needs scrutiny too. Generated text can become a query, a command, a web page, or an input to another application. Software should validate it for that intended use before passing it downstream. A plausible-looking response is not evidence that it is safe to execute. [OWASP's guidance on output handling](https://genai.owasp.org/llmrisk/llm052025-improper-output-handling/)

The integrations deserve the same attention. A connector, package, or external service becomes part of the system we depend on. We need to know who maintains it, what access it needs, and how changes reach our environment. Calling it an AI integration does not make ordinary software security work disappear.

## Enforce the Boundary Outside the Agent

An agent should not be the only thing responsible for enforcing its own limits. The systems around it need to control what it can reach and which actions it can execute, even when the model follows the wrong instruction.

NVIDIA's September 2026 Open Agent Safety Platform announcement provides one example of this approach. OpenShell applies runtime policies outside the agent process. Sentry adds an independent monitoring and enforcement layer using BlueField hardware. OpenShell can also run without that hardware. These are capabilities described by NVIDIA, and teams still need to evaluate them in their own environments. [NVIDIA's agent safety platform](https://www.nvidia.com/en-us/solutions/ai/agent-safety/)

That is the kind of control I want teams to investigate. Show me what gets blocked, what continues downstream, and what the business loses while the restriction is in place. Then show me how we recover. A product can provide the mechanism. We still own the decision about how to use it.

## Demonstrate the Containment Plan

Before production, I want more than a written statement that the service can be disabled. I want a demonstration of what happens when we use that control.

For a consequential workflow, the team should be able to show that new actions stop, explain the status of work already submitted, and identify the business functions affected. If we intend to keep a restricted service running, we need evidence that the restriction holds.

The response also needs a person with authority to act. A technically sound control is less useful if everyone is waiting to learn who is allowed to use it. That person needs enough context to judge the immediate harm against the downstream impact of intervention.

As we contain the problem, we should preserve enough evidence to understand it without creating unnecessary copies of sensitive information. We will examine incident response in more depth later in the guide. At this stage, the architecture needs to make a workable response possible.

Resuming service deserves a decision too. The original symptom disappearing does not prove the underlying problem is resolved. We need to understand what changed, what remains restricted, and what evidence supports restoring the capability.

## Closing Part II: Guardrails We Can Use

Across Part II, we have established who makes decisions, how spending is controlled, how we discover AI already in use, and how authority is assigned. Security brings those decisions into contact with failure, misuse, and behavior we did not anticipate.

These responsibilities connect. Discovery can reveal an integration that needs an owner. That owner needs to understand its costs, data access, and business purpose. If the integration starts behaving unexpectedly, somebody needs both the authority and the means to intervene.

I want teams to experiment and deliver useful capabilities. That becomes easier to support when we understand the risks we are accepting and have practical ways to respond. A policy nobody can apply will not help much during a difficult decision. Neither will a shutdown procedure nobody has tried.

Part III moves into building the capabilities themselves: models, context, retrieval, tools, and the internal platform. We can approach that work with a clearer understanding of who owns the outcome and how we keep control as the system becomes more capable.
