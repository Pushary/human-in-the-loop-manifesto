# Human-in-the-Loop for AI Agents

Ten principles for staying in control of agents that act on their own.

---

A year ago "AI agent" mostly meant a chatbot that could call a function. Now it means a process that edits your codebase, runs shell commands, opens pull requests, hits production APIs, and spins up more agents to help, often three or four at once, usually while you are doing something else.

Almost all the effort so far has gone into making these agents more capable. Very little has gone into the part that actually keeps you out of trouble: the moment between an agent deciding to do something and it happening. That is the window where a person can say no, or not yet, or fine, go ahead. On most setups there is nothing in that window. There is a log nobody reads and a terminal you are supposed to be watching.

This is an attempt to write down what belongs there. It is not a product pitch and it is not tied to any one tool. These are the rules I keep landing on after watching agents do things their owners did not expect. Keep the ones that hold up. Argue with the rest.

---

## The Principles

### 1. Autonomy is a dial, not a switch

An agent reading a file and an agent dropping a database table should not get the same answer. How much freedom an agent gets should track how bad it is when it gets the action wrong. Most tools treat this as one on/off setting for the whole agent, which is exactly how you end up either confirming everything by hand or confirming nothing and hoping.

### 2. Gate the action, not the agent

Whether you trust a call has almost nothing to do with which model made it. Claude, GPT, some 7B model on a laptop, they will all run `git push --force` against the wrong branch if you let them. What matters is the action and its arguments. Write the rules against those and they hold no matter which agent is driving, and no matter how many you are running at once.

### 3. Approve by exception

Ask a human to confirm everything and they stop reading the confirmations. By the twentieth "Allow npm install?" the yes is a reflex, and that same reflex sails right through the one prompt that actually mattered. Let the policy wave through the boring stuff, hard-block the obviously bad stuff, and only pull a person in for the cases in the middle, where judgment is the whole point.

### 4. Attention is the budget you are spending

Every notification costs a little of the user's attention, and that account only goes down. Ping someone each time an agent finishes a file and they will mute you within the hour, which means they also mute the alert that a deploy just failed. Send fewer. Make the ones you do send worth looking up from what you were doing.

### 5. Reach the human where they are

If approving an action means being at your desk with the right terminal open, then in practice nobody approves anything. They either babysit the agent, which defeats the purpose, or they wander off and let it run unwatched, which is worse. People should be able to approve, deny, or answer a question from their phone, from a meeting, from the kitchen.

### 6. Fail closed

Sooner or later the approval request will not get through. Phone is off, network is down, the person is asleep. When that happens the action waits. It does not proceed. An agent that fired a risky command because the timeout ran out and nobody said no was never really being supervised. And if "wait for approval" keeps turning into "blocked forever," that is a hint the policy is asking about things it should have just decided on its own.

### 7. One policy, every agent

Nobody can hold five different permission setups in their head. Once each tool keeps its own config, in its own format, in its own place, you have lost the thread on what any of your agents can actually do. Write the rules once, apply them everywhere. If you cannot say what all your agents are allowed to do without opening five files, you do not really know.

### 8. Every decision leaves a record

Write down each decision as it happens: what was asked, who approved or denied it, and the ones the policy decided on its own without anyone looking. Especially those. The auto-approved actions are invisible by design, and they are the first thing you will go hunting for when something breaks and you are trying to reconstruct what the agent actually did.

### 9. The record has to outlive the session

A history that lives in terminal scrollback, or inside a process that has already exited, is not a record. When you have to account for what an agent did, to a customer, to your team, to someone holding a compliance checklist, "I think it was in the logs" does not hold up. The trail needs to survive the session and be something you can search and export later, not just squint at in the moment.

### 10. A human can always pull the cord

However much rope you hand your agents, keep one move that takes it all back at once. Not "reconfigure the policy," not "let the current step finish." Stop, everything, now. Most weeks you will never reach for it. The reason you can give an agent real autonomy in the first place is that you know you can take it back instantly when it goes sideways.

---

## Why this is suddenly a problem

For most of software's history, the thing doing the action and the person on the hook for it were the same. You typed the command, you owned the result. Agents pull those apart. The thing acting is fast and automated; the person responsible is slow and somewhere else. Everything above is about stitching the two back together across that distance, without making the human sit in the loop for every trivial step.

There are two ways to blow it. Lock it down too hard and people drown in prompts, learn to rubber-stamp them, and eventually set the agent to "don't ask" just to make it stop, which is the opposite of what you wanted. Leave it too loose and you learn what your agents have been up to from the incident review. The principles here are the narrow path in between: automate enough that humans are not the bottleneck, keep enough control that they are still in charge.

## How to actually do it

You do not need a specific vendor for any of this. It comes down to four pieces that have to work together:

- A **policy engine** that decides, per action, whether to allow, deny, or ask, and that you configure once across every agent.
- An **approval path** that reaches a human wherever they are, and that blocks when it cannot reach them.
- An **audit log** that captures every decision and that you can still pull up months later.
- A **kill switch** that stops any agent on demand.

Build it on your own infrastructure, wire it together from open-source parts, or buy something that does all four. The real test is not what you are running. It is whether you can answer, right now, for any agent you have going: what is it allowed to do, who gets asked when it wants more, where is that written down, and how do I stop it.

---

## Contributing

These are meant to be argued with. If one is wrong, soft, or missing, open an issue or a PR and make the case. The point is a description of agent oversight that the people doing it actually agree with, not a frozen text.

## License

Released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Copy it, quote it, adapt it, translate it. A link back is appreciated.

---

<sub>Maintained by the team at [Pushary](https://pushary.com). Pushary is one way to do all of this: one permission policy across your agents, approvals on your phone, an audit trail you can export, and a kill switch. You do not need it to follow anything above.</sub>
