---
title: "Prompt guardrails are phishing training for agents"
date: 2026-11-17 06:00:00 +0000
tags: [security, agents, prompt-injection]
description: >-
  Prompt guardrails do for an AI agent what awareness training does for an
  employee: they are a layer worth having and a bad thing to depend on. The
  rest of the employee security playbook applies to agents too.
---

Every company that takes email security seriously runs phishing tests. Nobody
who runs them believes they stop phishing. They lower the odds that someone
clicks, and they tell you how big the remaining odds are. The protection that
has to hold comes from somewhere else.

Prompt guardrails deserve the same status. System prompt instructions,
injection classifiers and models trained to resist manipulation all belong in
the stack, and none of them should be the thing standing between an attacker
and your data.

## How we protect people from email

No security team picks one defence against phishing. A typical setup has four
layers:

1. **Filtering.** Spam and reputation filters drop most hostile mail before
   anyone sees it.
2. **Scanning.** Attachments are detonated in a sandbox and links are rewritten
   and checked when they are clicked.
3. **Awareness.** People are trained to spot a suspicious message and are
   tested with simulated phishing.
4. **Hard controls.** When someone clicks anyway, multi-factor authentication,
   restricted permissions, endpoint protection and payment approvals limit
   what the attacker gets.

The third layer is the only one that lives inside the person's head. It is
also the only one that is probabilistic by nature. The click rate in a
phishing simulation falls with training and never reaches zero, because a
well-made message on a busy day gets through to anyone.

That is why the fourth layer exists. It is designed on the assumption that the
third one has already failed.

## Guardrails are the awareness layer

Prompt guardrails sit in the same place for an agent. They shape the judgement
of the thing reading the content, whether that content is a web page, an
email, a ticket or the output of a tool. A model told to ignore instructions
in documents it reads, and trained to recognise injection attempts, falls for
fewer of them. A red-team evaluation then gives you a number, exactly as a
phishing simulation does.

That number is not zero either, and for the same reason. The attacker gets
unlimited attempts at writing the message, and the defender relies on the
reader making the right call every time.

Seen this way, the question to ask about any agent deployment is the one you
would ask about an employee: what happens when it does get fooled?

| For employees | For agents |
| --- | --- |
| Spam and reputation filtering | Filtering and sanitising untrusted input before the model reads it |
| Attachment and link scanning | Classifiers that scan retrieved content and tool output for injections |
| Awareness training and phishing tests | Prompt guardrails, model training and red-team evaluations |
| Least privilege and role-based access | Tools and credentials scoped to the task in hand |
| Endpoint and SaaS policy that blocks unapproved actions | Deterministic checks outside the model that block unapproved actions |
| Insider threat monitoring | Logging and anomaly detection on what the agent does |

## Borrowing the rest of the playbook

We have spent decades learning how to let fallible people work with sensitive
systems. Most of what we learned transfers to agents with little change.

### Treat the agent as an insider

An agent holds credentials, reads internal data and acts inside the
perimeter. That makes it an insider, and insider threat thinking applies. An
insider does not have to be malicious to do damage. One who has been tricked
or coerced is dangerous in the same way, and their activity looks legitimate
because it uses legitimate access.

A successfully injected agent is exactly that: a trusted identity acting on
someone else's instructions. So log what it does, establish what normal looks
like, and alert on departures from it, such as a support agent that suddenly
reads the whole customer table or sends data to a domain it has never
contacted.

### Let it perform only the correct tasks

Employees get access according to their role. An accountant cannot deploy to
production, and a developer cannot approve payments. Agents are often given
the opposite: one broad token that covers everything they might ever need.

Scope the agent to the task it is doing now. An agent summarising an inbox
needs to read mail and has no need to send it. An agent triaging tickets has
no need for the billing API. Every permission that is absent is an attack that
cannot succeed, however well the injection is written.

### Put hard guardrails outside the model

Awareness training asks an employee not to install unapproved software. A
managed laptop makes it impossible. Mature organisations do both, and it is
the second that they rely on.

The equivalent for agents is a control that runs in ordinary code, outside the
model, where no prompt can argue with it:

- an allowlist of tools and of the arguments they accept,
- network egress limited to known destinations,
- data loss prevention checks on what leaves the system,
- human approval before irreversible actions such as payments, deletions and
  external messages.

These controls are deterministic. They give the same answer however persuasive
the injected text is, which is the property the awareness layer can never
have.

## Where the comparison stops

Agents differ from employees in two ways that make the hard controls matter
more.

The first is scale. A phishing message that fools one employee has fooled one
employee. An injection that works on an agent works on every copy of that
agent, every time, until something changes.

The second is speed. A person who has been tricked takes minutes or hours to
do damage, and there are moments where a colleague can notice. An agent
completes the whole chain in seconds.

Both differences shorten the window in which detection can help, and both
move weight from the awareness layer to the layers that do not depend on
judgement.

## The short version

Keep the prompt guardrails. Measure them the way you measure phishing click
rates, and expect the result to stay above zero. Then build the rest of the
stack as you would for any employee with access to things that matter: filter
what comes in, limit what the agent can do, monitor what it does, and enforce
the rules that must never break in a place the model cannot reach.
