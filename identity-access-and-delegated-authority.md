# Identity, Access, and Delegated Authority

I regularly get requests from application owners for the Owner role in Azure. I understand the connection. They own the application, and there is a role called Owner. Sounds reasonable. Unfortunately, the role does not mean "the person we call when this application breaks."

Azure Owner includes broad resource management permissions and the ability to assign access to others within its assigned scope. Being responsible for an application does not automatically mean someone needs that authority. I suspect some people asking for the role do not realize everything that comes with it.

For those requests, Contributor is my ceiling, and even that is a lot of permission. That is an upper boundary for those requests, not the starting permission for every application team. We should still use a narrower role and scope wherever they support the work. It allows broad resource management within its assigned scope, although the role itself does not allow Azure RBAC role assignments. Removing the ability to grant access does not remove the ability to make a consequential change. [Microsoft's role definitions explain the distinction.](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/privileged)

I would love to say every migration starts with perfectly defined permissions for every task. That has not been my experience. During migrations from on-premises infrastructure to the cloud, we have faced hard deadlines and given application teams Contributor access so they could configure their applications and keep the migration moving.

That was a tradeoff. We understood that the access was broader than the long-term requirement and made a decision to proceed. Once the migrations were complete, we moved into the IAM cleanup work, narrowing permissions toward a least-privilege model.

There is a lesson in both halves of that story. Sometimes leadership means understanding the risk, making a decision, and moving. It also means following through when the circumstances that justified the decision have changed. "We needed it for the migration" cannot explain an access assignment forever.

For future exceptions, I want that follow-through built into the decision: someone responsible for revisiting the access and a clear point when the review happens. Otherwise, "temporary" starts looking suspiciously like a permanent architecture choice.

## When Software Gets the Keys

AI brings another participant into that same access discussion. An assistant might help someone understand an application problem, recommend a configuration change, or execute the change through a connected tool. Those are different responsibilities, and they need different permissions.

Suppose an application owner asks an assistant to investigate a failed deployment. Reading deployment logs may be enough to diagnose the problem. Changing infrastructure requires additional authority. Granting another identity access is a different decision again. One request for help should not quietly authorize all three.

The application owner may have broad access because of responsibilities that extend well beyond this task. That does not mean the assistant needs all of it. We need to decide what the assistant can do, which identity it uses, and where the platform will enforce those limits.

The same judgment applies to the identities behind the automation. Giving an AI tool a powerful service account because it makes the integration easier is still a permissions decision. The model's instructions do not reduce what that account is technically allowed to do.

My starting point is the same as it is with the person requesting Owner: tell me what work needs to happen. Then we can work out the access needed to do it.

## Signing In Is Only the Beginning

Single sign-on answers an important question about who is using a service. It does not settle what that person, or an assistant working for them, should be allowed to do.

A user might be entitled to use the company's AI platform without being entitled to every document connected to it. They might be able to ask for deployment advice without being able to deploy to production. Access to the front door does not grant access to every room.

That distinction matters when we connect enterprise information. If an assistant retrieves content through a broadly privileged connection, we need a reliable way to enforce the requesting user's access before that content reaches the model. Asking the model to keep certain information secret after we have already supplied it is a poor substitute for controlling retrieval.

The service catalog and approval process can establish who receives access to an AI service. The application still has to enforce what each person can see and do inside it.

## Decide Whose Authority Is Being Used

When an assistant calls another system, that call runs under some identity. It may act with permissions delegated by the user, or it may use a separate identity assigned to the application or workload.

That choice affects accountability and the consequences of a mistake. Acting on behalf of a user can help preserve that user's access boundaries, but the application should still limit what it requests and what operations it offers. A user's permission to do something does not mean every assistant they use needs that same capability.

A workload identity is useful for background work that does not depend on a person staying signed in. It also needs its own clearly defined purpose. An assistant that checks deployment status should not share the same broad identity as an automation service that manages the entire environment.

Where supported, I would use managed identities or other mechanisms that provide short-lived credentials, rather than copying long-lived secrets between integrations. That reduces the credentials we have to distribute and rotate. The identity still needs permissions limited to the work it performs.

I want to be able to explain an action in ordinary language: this person requested it, this service executed it, and these permissions allowed it. If the explanation disappears into a shared account used by six unrelated integrations, we have made both access management and incident investigation harder.

## Make Approval Mean Something

Consider the failed deployment again. The assistant reads the logs, identifies a likely configuration problem, and proposes a correction. Up to that point, it has been investigating. Applying the correction introduces a new consequence.

An approval should make that consequence visible. "Allow the assistant to continue?" tells me very little. I want to see the affected application, the environment, the proposed change, and the expected impact. The person approving also needs the authority to make that decision.

Approval should apply to the proposed action. If the assistant changes the target or decides a different operation is necessary, the earlier approval should not become permission for whatever comes next.

This does not mean putting a human confirmation in front of every harmless step. People will stop paying attention if we interrupt them constantly. We should reserve meaningful approvals for meaningful decisions and allow routine work within boundaries we have already established.

The tool or service executing the change must enforce those boundaries. A sentence in a prompt saying "always ask first" is not sufficient control over a production operation.

## Give Temporary Access an Ending

The migration example applies here too. There may be a legitimate reason to grant additional access for a particular task. The important questions are how much, for how long, and who is responsible for removing it.

Where the platform supports it, time-limited access can help turn that intention into something enforceable. Where it does not, someone needs an assigned review and a clear trigger. A completed migration, a finished investigation, or the end of a vendor engagement should cause us to revisit the permissions granted for that work.

The same applies when an AI experiment becomes a production service. The identity used to get a prototype working deserves another look before the service becomes a business dependency. Credentials copied into a demonstration and permissions granted during troubleshooting should not become permanent through neglect.

User provisioning and deprovisioning are part of this lifecycle, but they are not the whole lifecycle. Removing someone's access to an AI application does not necessarily retire the separate identities and integrations that application uses. Those need owners and retirement decisions of their own.

## Test the Boundaries Too

A successful demonstration usually shows that the assistant can complete the task. I also want evidence that it cannot complete a task outside its authority.

For our deployment assistant, that might mean confirming it cannot retrieve another team's restricted logs, change an unapproved environment, or execute a proposed change after approval has expired. These checks tell us whether the controls hold when the request crosses a boundary.

When access is denied, the response should help the user understand the approved next step. It should not encourage the assistant to hunt for another credential or a more powerful route around the restriction.

Useful automation depends on permissions that let work move. Sustainable automation depends on knowing where those permissions stop. That is the judgment behind every Owner request, every migration exception, and every AI integration we allow to act on the company's behalf.
