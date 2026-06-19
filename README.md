# Human-in-the-Loop for AI Agents

**Ten principles for keeping a human in control of autonomous AI agents.**

---

AI agents now run real commands. They edit code, move money, send email, touch production, and call other agents, often several at once, often while no one is watching. The model is no longer the interesting part. The interesting part is everything that happens between an agent deciding to act and the action taking effect.

That gap is where oversight lives, and most teams have nothing there. They have logs they never read, a terminal they have to babysit, and a vague hope that the agent will not do anything stupid while they are at lunch.

This is a manifesto for the missing layer: the controls that let a person stay responsible for what their agents do without sitting on top of every keystroke. It is not about a product. It is a description of what good oversight looks like, written so you can hold any tool, including your own, against it.

The principles are opinionated on purpose. Take the ones that are right and argue with the rest.

---

## The Principles

### 1. Autonomy is a dial, not a switch

An agent's freedom to act should scale down as the consequences of its actions scale up. Reading a file is not deleting a database. Drafting a message is not sending it. Treat low-consequence and high-consequence actions differently, by default, or you will end up either blocking everything or trusting everything. Both are failures.

### 2. Gate the action, not the agent

Trust is not a property of which model or vendor is running. A frontier model and a small local one are equally capable of running `rm -rf` on the wrong directory. Govern by what the action does and what arguments it carries, not by which agent asked. The same rule should hold whether the call came from Claude Code, Codex, Cursor, a cron job, or something you have not heard of yet.

### 3. Approve by exception

Asking a human to confirm everything trains them to approve everything. The tenth identical prompt gets the same reflexive yes as a genuinely dangerous one. Auto-approve what is provably safe, hard-block what is forbidden, and spend human judgment only on the consequential middle. A good policy makes most decisions so the human can make the few that matter.

### 4. The human's attention is the scarce resource

Every notification spends attention, and attention does not refill. An oversight layer that interrupts constantly is worse than none, because people learn to swipe it away, and then they swipe away the one that mattered. Notify on consequence. Stay silent the rest of the time. Silence is a feature.

### 5. Reach the human where they are

Oversight that requires sitting at a terminal is not oversight. It is a leash, and the first thing people do with a leash is put it down. If a human has to be in front of the machine to approve anything, then in practice nothing gets approved and the agent either stalls or runs unsupervised. A person should be able to approve, deny, or answer from wherever they are, including their phone, including away from the desk.

### 6. Fail closed

When the approval path is unreachable, the safe default is to stop, not to proceed. An agent that runs a risky command because no one answered in time is an agent with no oversight at all; the gate was theater. Unreachable means blocked. If that is annoying, the fix is a better policy, not a more permissive failure mode.

### 7. One policy, every agent

Humans cannot reason about safety if every tool has its own settings buried in its own config. Define the rules once and apply them to every agent a person or team runs. Five agents with five different, half-remembered permission setups is the same as no controls, because no one can hold the whole picture in their head. Fragmented control is not control.

### 8. Every decision leaves a record

Who approved what, when, and why is not metadata to be sampled or dropped under load. It is the point. Every approval, every denial, and every automatic decision should be written to an append-only record at the moment it happens, including the ones that were auto-approved. The decisions you did not see are exactly the ones you will want to look up later.

### 9. The record outlives the session

A trail you cannot export, search, or hand to someone six months from now does not exist. When an agent does something you need to explain, to a customer, to a regulator, to your own team, "it scrolled off in the terminal" is not an answer. Audit is durable, portable, and tamper-evident, or it is decoration.

### 10. A human can always pull the cord

No matter how autonomous the fleet, one person must be able to halt any agent immediately, without negotiating with it. The kill switch is not an admission that the system failed. It is the feature that lets you grant autonomy in the first place, because you know you can take it back in one move. Hope you never use it. Always have it.

---

## Why this matters now

For most of software history, the thing taking the action and the person responsible for it were the same. You ran the command, you owned the result. Agents break that. The thing taking the action is autonomous and fast; the person responsible is human and elsewhere. Oversight is what reconnects responsibility to action across that gap.

Get it wrong in the cautious direction and you confirm-prompt your team into ignoring every prompt, then turn the agent loose to make it stop bothering you. Get it wrong in the permissive direction and you find out what your agents did from the incident, not from the approval. The principles above are an attempt to get it right: enough automation that humans are not the bottleneck, enough control that humans are still in charge.

## How to put it into practice

You do not need a specific vendor to follow these principles. You need four things working together:

- **A policy engine** that decides, per action, whether to allow, deny, or escalate, and that you configure once across every agent (Principles 1, 2, 3, 7).
- **An approval channel** that reaches a human wherever they are and fails closed when it cannot (Principles 4, 5, 6).
- **An audit log** that records every decision append-only and lets you export it later (Principles 8, 9).
- **A kill switch** that can stop any agent immediately (Principle 10).

Assemble that from your own infrastructure, from open-source parts, or from a tool that does all four. The test is not what you bought. The test is whether you can answer, right now, for any agent you run: what is it allowed to do, who gets asked when it wants more, where is that written down, and how do I stop it.

---

## Contributing

These principles are meant to be argued with. If one is wrong, incomplete, or missing, open an issue or a pull request and make the case. The goal is a description of agent oversight that practitioners actually agree with, not a fixed text.

## License

This manifesto is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Share it, quote it, adapt it, translate it. Attribution appreciated.

---

<sub>Maintained by the team at [Pushary](https://pushary.com), which is one implementation of these principles: one permission policy across every AI agent, phone approvals, an immutable cross-agent audit trail, and a kill switch. You do not need Pushary to follow the manifesto, and the manifesto does not need Pushary to be right.</sub>
