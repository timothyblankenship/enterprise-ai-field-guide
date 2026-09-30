# Finding and Owning the AI Estate

Recently, I was working in Microsoft Entra when I noticed a surprising number of meeting recorder applications. That was not where I expected to get a better picture of our AI footprint, but there it was. Discovery sometimes starts with a project plan. Sometimes it starts with, "Hang on. What are all these?"

Along the way, we learned that Entra allows standard member users to register applications by default and manage the applications they create. We had not realized that. Application registration and permission to access company data are separate things, so that setting alone did not explain how the recorders arrived or what they could access. Disabling application registration would not, by itself, remove existing integrations or revoke permissions already granted. Finding the applications gave us a starting point, not the whole story. [Microsoft documents how these different paths work.](https://learn.microsoft.com/en-us/entra/identity-platform/how-applications-are-added)

That experience is why I would not start an AI inventory with only a list of approved projects. That list tells us what went through our process. We still have to find out what else is there.

Seeing a meeting recorder in a directory does not establish that sensitive conversations were recorded or that anyone did something wrong. It does give us something concrete to investigate: who uses it, what access it has, what business need it serves, and who owns the decision to keep it. Until we understand those things, we have a list of application names. We do not yet understand the environment we are responsible for.

## Start With What Is Actually There

An AI initiative might arrive with an executive sponsor, a budget, and an architecture review. A meeting assistant might arrive because someone wanted to pay attention during a call instead of taking notes. Both belong in our understanding of the environment, even though only one came with a kickoff meeting.

AI can also appear inside software the company already uses. An existing vendor adds a feature, someone enables it, and a familiar application now handles information differently. We may not have bought a new product at all.

That makes discovery a shared effort. Identity records can reveal connected applications. Purchasing and expense records can reveal subscriptions. Cloud billing can reveal model services. Conversations with developers and business teams can reveal experiments that have not yet become formal projects.

Each source tells part of the story. A subscription does not prove active use, and an application entry does not tell us everything it can access. We need to connect the evidence before drawing conclusions.

## Make It Easy to Tell Us

I believe in giving people room to experiment. It would be hard to defend that position and then act offended every time someone finds a useful tool before my team does.

People generally have work they are trying to get done. If a tool helps them capture meeting notes, summarize documents, or stop copying information between systems, that need deserves a hearing. We can take the need seriously while still deciding that a particular tool or use is unacceptable.

The conversation goes better when we start by understanding the work. Someone who expects a lecture may give us the shortest possible answer. Someone who believes we can help is more likely to explain what they use, why they chose it, and where the approved options fall short.

That does not remove accountability. It gives us a better chance of making decisions with complete information. If the approved path is so difficult that people routinely find another route, we should examine our process along with their choices.

## An Inventory Should Help Us Decide

I do not want an inventory that becomes another thing people update because someone keeps sending reminders. It needs to help us make decisions.

For each AI service or use case, we need enough information to understand its purpose, its business owner, the information it handles, the access it has, and how it is paid for. We also need to distinguish an experiment from something a team now depends on every day.

Unknowns belong in the record too. "Permissions not yet reviewed" is useful information. An empty field that everyone assumes somebody else checked is not.

The depth of review should follow the consequences. A tool used to improve public marketing copy presents a different situation from an assistant that can read internal conversations or take action in a business system. Treating every entry identically can consume time while the more consequential uses wait for attention.

The inventory earns its keep when it helps us decide what to approve, investigate, consolidate, restrict, or retire.

## Put a Person Behind the Entry

An owner needs to be more than the person whose name was available when somebody filled out the form.

The business owner should understand why the capability exists and whether it is still needed. Whoever operates the service needs responsibility for its configuration, access, support, and changes. Those responsibilities may sit with different people, but the handoff needs to be clear.

This connects directly to the financial ownership we discussed earlier. A central AI cost center can make spending visible, but it does not explain every business use or make FinOps the operational owner of every application. We still need to know which team receives the value and who can make decisions about that use.

Ownership becomes especially important when people move roles or leave. A useful experiment can quietly become a business dependency while the person who understood it moves on. By then, deleting an unfamiliar application may be just as poorly informed as leaving it alone.

## Keep Discovery Connected to Daily Work

A one-time inventory starts aging as soon as we finish it. New services appear, permissions change, and existing products gain capabilities.

The sustainable approach is to make normal work improve the picture. An approved service request should establish an owner. A purchase should connect spending to a business purpose. A material change in access or data use should trigger a review. Retirement should close out access and costs as well as the inventory entry.

We will still need periodic checks against identity, purchasing, and cloud records. Those checks help us find what our normal processes missed. They should also help us improve those processes.

My Entra experience is a useful reminder that familiarity with a platform does not mean we have a complete picture of everything inside it. Sometimes the most valuable discovery is simply noticing something unexpected and taking the time to understand it.

That is where this work starts. Before we can make sensible decisions about AI, we need to know what we are making decisions about.
