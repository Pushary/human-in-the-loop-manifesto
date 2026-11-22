# Human-in-the-Loop for AI Agents

Twelve principles for keeping a person in control of AI agents that act on their own: when an agent should ask, who it should ask, and what happens when nobody answers.

**The short version**

1. Autonomy is a dial, not a switch.
2. Gate the action, not the agent.
3. Approve by exception.
4. Attention is the budget you are spending.
5. Reach the human where they are.
6. If the ping goes unanswered, call.
7. Warn the person. Don't overrule them.
8. Fail closed.
9. Ask the person who owns the decision.
10. One policy for every agent and every teammate.
11. Every decision leaves a record your team can read.
12. A human can always pull the cord.

---

A year ago "AI agent" mostly meant a chatbot that could call a function. Now it means a process that edits your codebase, runs shell commands, opens pull requests, hits production APIs and spins up more agents to help, often three or four at once, usually while you are doing something else. And more and more of them work for a team, or for somebody else's customer.

Almost all the effort so far has gone into making these agents more capable. Very little has gone into the part that keeps you out of trouble: the moment between an agent deciding to do something and it happening. That is the window where a person can say no, or not yet, or fine, go ahead. On most setups there is nothing in that window. There is a log nobody reads and a terminal you are supposed to be watching.

This is an attempt to write down what belongs there. It is not tied to any one tool. These are the rules I keep landing on after watching agents do things their owners did not expect, and after getting the "waiting for your permission" screen back from lunch more times than I want to count. Keep the ones that hold up. Argue with the rest.

---

## The principles

### 1. Autonomy is a dial, not a switch

An agent reading a file and an agent dropping a database table should not get the same answer. How much freedom an agent gets should track how bad it is when it gets the action wrong. Most tools treat this as one on/off setting for the whole agent, which is exactly how you end up either confirming everything by hand or confirming nothing and hoping.

### 2. Gate the action, not the agent

Whether you trust a call has almost nothing to do with which model made it. Claude, GPT, some 7B model on a laptop, they will all run `git push --force` against the wrong branch if you let them. What matters is the action and its arguments. Write the rules against those and they hold no matter which agent is driving, and no matter how many you are running at once.

### 3. Approve by exception

Ask a human to confirm everything and they stop reading the confirmations. By the twentieth "Allow npm install?" the yes is a reflex, and that same reflex sails right through the one prompt that mattered. Let the policy wave through the boring stuff, hard-block the obviously bad stuff, and only pull a person in for the cases in the middle, where judgment is the whole point.

### 4. Attention is the budget you are spending

Every notification costs a little of someone's attention, and that account only goes down. Ping someone each time an agent finishes a file and they will mute you within the hour, which means they also mute the alert that a deploy just failed. Send fewer. Make the ones you do send worth looking up from what you were doing.

### 5. Reach the human where they are

If approving an action means being at your desk with the right terminal open, then in practice nobody approves anything. They either babysit the agent, which defeats the purpose, or they wander off and let it run unwatched, which is worse. People should be able to approve, deny or answer from their phone, from a meeting, from the kitchen.

### 6. If the ping goes unanswered, call

A push is easy to miss. The phone is face down, it sank under three group chats, you are at lunch. Meanwhile the agent sits on one question for forty minutes and gets nothing done. If a question only you can answer has been waiting a minute or two, the next step should be louder: ring the phone.

The call has to be worth picking up. It should say which agent is asking and what it wants to run, let you ask what that means, and take yes, no, or what to do instead. A yes should be read back before it counts. And it should only ring the number the person confirmed, in the hours they picked. Nobody wants their agent calling at 3 a.m. about a lint fix.

### 7. Warn the person. Don't overrule them.

When the person allowed to decide is on the phone, tell them plainly what is dangerous about the step and let them decide. A call or a notification that ends with "you'll have to approve this at your desk" is no better than no call at all. The agent stays stuck and the person learns to ignore you.

Some teams will want certain steps kept off the phone entirely, say anything touching production. That is a fine rule, but it should be a rule the team wrote down, not a default the tool imposed on everyone.

### 8. Fail closed

Sooner or later the request will not get through. Phone is off, the call goes to voicemail, the person is asleep. When that happens the action waits. It does not proceed. An agent that fired a risky command because a timeout ran out and nobody said no was never really being supervised. And if "wait for approval" keeps turning into "blocked forever", that is a hint the policy is asking about things it should have decided on its own.

### 9. Ask the person who owns the decision

In a team, the agent should ask the person who started it. A shared channel where everyone assumes somebody else answered is how a question sits for an hour.

Some steps belong to someone other than the person running the agent. A deploy might belong to whoever is on call, a payment to finance. Route those by rule, to a role or a named person, and keep the rule somewhere the whole team can see it.

And when an agent acts for someone else's customer, a refund, an email to their list, a change to their account, the yes belongs to that customer. A model can guess what I would allow on my own laptop. It cannot consent for a stranger.

### 10. One policy for every agent and every teammate

Nobody can hold five different permission setups in their head. Once each tool keeps its own config, in its own format, in its own place, you have lost the thread on what any of your agents can actually do. That gets worse when ten people each run three of them. Write the rules once and apply them everywhere. If you cannot say what all your agents are allowed to do without opening five files, you do not really know.

### 11. Every decision leaves a record your team can read

Write down each decision as it happens: what was asked, who approved or denied it, and the ones the policy decided on its own without anyone looking. Especially those. The auto-approved actions are invisible by design, and they are the first thing you will go hunting for when something breaks.

A history that lives in terminal scrollback, or inside a process that has already exited, is not a record. It has to outlive the session, be searchable and exportable, and answer the question a team actually asks after an incident: who said yes? Then read it together. When the same question keeps coming back with the same answer, turn that answer into a rule so nobody gets asked again.

### 12. A human can always pull the cord

However much rope you hand your agents, keep one move that takes it all back at once. Not "reconfigure the policy", not "let the current step finish". Stop, everything, now. Most weeks you will never reach for it. The reason you can give an agent real autonomy in the first place is that you can take it back the moment it goes sideways.

---

## Why this is suddenly a problem

For most of software's history, the thing doing the action and the person on the hook for it were the same. You typed the command, you owned the result. Agents pull those apart. The thing acting is fast and automated; the person responsible is slow and somewhere else. Everything above is about stitching the two back together across that distance, without making the human sit in the loop for every trivial step.

In 2026 most of the big AI labs shipped some way to approve their own agent from a phone. That helps, and it also shows the gap: each one only answers for its own agent, it only asks the developer who pays for it, and people rarely run just one agent. Teams run many. Products built on agents act for customers no lab will ever talk to.

There are two ways to blow it. Lock it down too hard and people drown in prompts, learn to rubber-stamp them, and eventually set the agent to "don't ask" just to make it stop, which is the opposite of what you wanted. Leave it too loose and you learn what your agents have been up to from the incident review. The principles here are the narrow path in between.

## How to actually do it

You do not need a specific vendor for any of this. It comes down to five pieces that have to work together:

- A **policy engine** that decides, per action, whether to allow, deny or ask, configured once across every agent and every teammate.
- An **approval path** that reaches a person wherever they are, escalates from a notification to a phone call when the first ask is missed, and waits when it cannot reach anyone.
- **Routing** that sends each question to the person who owns that decision: whoever started the agent, a named teammate, a role, or the customer the agent works for.
- A **record** of every decision, who made it and why, that you can still pull up months later.
- A **kill switch** that stops one agent, or all of them, on demand.

What you run matters less than whether you can answer, right now, for any agent you have going: what is it allowed to do, who gets asked when it wants more, what happens if they don't answer, where is that written down, and how do I stop it.

---

## Questions people ask

**What is human-in-the-loop for AI agents?**
It means a person approves, denies or redirects an AI agent's action before the action happens, for the steps where a mistake would be costly. Low-risk actions run on their own. High-risk ones wait for a yes.

**When should an AI agent ask a human?**
When the action is hard to undo or reaches outside the agent's sandbox: pushing to a shared branch, deleting data, deploying, spending money, contacting customers. Reading files and running tests usually should not need a person. Principles 1 to 3 cover this.

**What should happen if the person misses the approval notification?**
Escalate. After a minute or two, call their phone, explain what the agent wants and take the answer on the call. If the call is missed too, the action keeps waiting. It never goes ahead by default. See principles 6 and 8.

**Should someone be able to approve a risky step on a phone call?**
Yes, if they are the person allowed to decide. The call should name the danger first and read the yes back. A team can still require that some steps are only approved at a keyboard, as a rule it writes down. See principle 7.

**Who should approve an AI agent's action in a team?**
The person who started the agent, by default. Steps that belong to someone else, like a production deploy or a payment, go to a named teammate or a role by rule. Everyone should be able to see afterwards who said yes. See principles 9 and 11.

**How do you keep a person in control of Claude Code, Codex or other coding agents?**
Use their permission hooks to send each tool call through one policy, ask a person only for the risky ones, reach that person on their phone, and record every answer. The same rules should apply to every agent you run.

**What about AI agents that act for customers?**
The customer is the one who should approve. A refund, an email to their contacts or a change to their account needs their yes, on their phone, with a record of who answered. The company running the agent cannot consent on their behalf.

**Do I need a specific tool to follow this?**
No. Build it on your own infrastructure, wire it together from open-source parts, or use something that does all of it.

---

## How Pushary does it

Pushary is one implementation, built by the people who maintain this document. You do not need it to follow anything above.

| Principle | In Pushary |
| --- | --- |
| 1 to 3, 10 | One permission policy across Claude Code, Codex, Gemini CLI, Hermes, Cursor and more: safe steps run, risky ones ask |
| 5 | [Approvals on your phone](https://pushary.com/ai-agents), your Mac's notch, the web or Slack |
| 6, 7 | [Phone calls](https://pushary.com/calls) when you miss the ping: the call explains the step, warns you about risky ones and takes yes, no or an instruction |
| 8 | Missed questions keep waiting. Nothing is approved by default unless you wrote that rule |
| 9 | [Pushary for Teams](https://pushary.com/teams): each teammate's agent asks them, and routing rules send a step to a role or a named teammate. [Partners](https://pushary.com/partners) put the question in front of their own customers |
| 11 | A shared record of every request, answer and who gave it |
| 12 | A kill switch that stops one session or every agent at once |

Read more about [human-in-the-loop for AI agents](https://pushary.com/human-in-the-loop).

---

## Changes

- **November 2026, version 2.** Added principle 6 (call when the ping is missed), 7 (warn, don't overrule) and 9 (ask the person who owns the decision, in a team or for a customer). Merged the two record principles into 11 and added team review. Added the questions section.
- **June 2026, version 1.** Ten principles.

## Contributing

These are meant to be argued with. If one is wrong, soft or missing, open an issue or a pull request and make the case. The point is a description of agent oversight that the people doing it agree with, not a frozen text.

## Citing

Use the "Cite this repository" button on GitHub, or: Aadil Ghani, *Human-in-the-Loop for AI Agents*, version 2, November 2026, https://github.com/Pushary/human-in-the-loop-manifesto.

## License

Released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Copy it, quote it, adapt it, translate it. A link back is appreciated.
