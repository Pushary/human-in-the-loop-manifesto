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

I run several coding agents at once, mostly Claude Code and Codex. They edit code, run shell commands, open pull requests and deploy, usually while I'm doing something else. More people now run agents like that for a whole team, or for their own customers.

The agents got good fast. The moment between an agent deciding to do something and actually doing it didn't get any better. That's where a person gets to say no, or not yet, or go ahead. On most setups it's a terminal you're supposed to be watching.

I started building Pushary after coming back from lunch to "waiting for your permission" one too many times. The first version only told me when an agent had finished. That turned out to be the wrong problem. The real one was the agent stuck halfway, waiting on a question I didn't know it had asked.

These are the rules I've ended up with since, and from watching agents do things their owners didn't expect. They aren't tied to any tool, including mine. If one of them is wrong, tell me.

---

## The principles

### 1. Autonomy is a dial, not a switch

An agent reading a file and an agent dropping a database table shouldn't get the same answer. The more damage a wrong step can do, the less freedom the agent should have to take it alone. Most tools give you one setting for the whole agent, on or off. So you end up confirming everything by hand, or confirming nothing and hoping.

### 2. Gate the action, not the agent

Whether a step is safe has almost nothing to do with which model wants to take it. Claude, GPT and some 7B model on a laptop will all run `git push --force` on the wrong branch if you let them. So write the rules against the command and its arguments. Then they hold no matter which agent is driving.

### 3. Approve by exception

Ask someone to confirm everything and they stop reading. By the twentieth "Allow npm install?" the yes is a reflex, and it goes straight through the one prompt that mattered. Let the policy allow the boring stuff and block the obviously bad stuff. Only ask a person about what's left in the middle.

### 4. Attention is the budget you are spending

Every notification spends a bit of someone's attention, and they don't get it back. Ping someone each time an agent finishes a file and they'll mute you within the hour. Then they miss the one about the failed deploy too. Send fewer, and make each one worth looking up for.

### 5. Reach the human where they are

If approving a step means sitting at your desk with the right terminal open, nobody approves anything. People babysit the agent, which defeats the point, or they walk off and let it run unwatched. You should be able to approve, deny or answer from your phone, wherever you are.

### 6. If the ping goes unanswered, call

A push notification is easy to miss. Your phone's face down, or you're at lunch. Meanwhile the agent has been sitting on one question for forty minutes. If a question only you can answer has waited a minute or two, ring the phone.

The call has to be worth picking up. It should say which agent is asking and what it wants to run, explain it if you ask, and take yes, no, or what to do instead. Read a yes back before it counts. Only ring the number the person confirmed, and only in the hours they picked. Nobody wants a call at 3 a.m. about a lint fix.

### 7. Warn the person. Don't overrule them.

The first version of our calls refused risky steps. It explained the step, then told you to go approve it at your desk. That call was useless. The agent stayed stuck, and I'd been interrupted for nothing.

Now the call says plainly what's dangerous about the step and lets you decide. If you're the person allowed to approve it, a phone call shouldn't take that away.

Some teams want certain steps kept off the phone, anything touching production for example. That's fine when the team chose it and wrote it down. It shouldn't be something the tool decides for everyone.

### 8. Fail closed

Sometimes the question won't get through. The phone's off, or the call goes to voicemail. Then the action waits. A risky command that ran because a timeout expired and nobody said no wasn't supervised at all.

If waiting keeps turning into "blocked forever", the policy is asking about things it should be deciding itself.

### 9. Ask the person who owns the decision

On a team, an agent should ask the person who started it. Post the question to a shared channel and everyone assumes someone else answered it.

Some steps belong to someone else. A deploy might belong to whoever's on call, a payment to finance. Route those by rule, to a role or a named person, and keep the rule where the whole team can see it.

When an agent works for someone else's customer, say issuing a refund or emailing their list, the yes belongs to that customer.

### 10. One policy for every agent and every teammate

Nobody can keep five permission setups in their head. Once every tool has its own config file, you've lost track of what your agents can actually do, and it gets worse when ten people each run a few. Write the rules once and use them everywhere, for every agent and everyone on the team.

### 11. Every decision leaves a record your team can read

Write down every decision when it happens: what was asked, who answered, and what the policy approved without asking anyone. The auto-approved ones matter most. Nobody looked at them, so they're the first thing you'll go looking for when something breaks.

Terminal scrollback isn't a record, and neither is a process that already exited. Keep it somewhere that outlives the session and that you can search, so you can answer the question every incident review starts with: who said yes?

Read it together now and then. When the same question keeps getting the same answer, make it a rule and stop asking.

### 12. A human can always pull the cord

Keep one button that stops all of your agents at once, in the middle of whatever they're doing. You'll rarely press it. It's still the reason I'm comfortable letting them run while I'm away.

---

## Why this is suddenly a problem

Whoever ran a command used to be the person on the hook for it. Agents split that. The agent acts in seconds, and the person who answers for it is in a meeting.

In 2026 most of the big AI labs shipped a way to approve their own agent from a phone. That helps. But each one only covers its own agent and only asks the developer paying for it. Hardly anyone runs just one agent anymore. Teams run lots of them, and products built on agents act for customers no lab will ever talk to.

There are two ways to get this wrong. Lock it down and people drown in prompts until they switch the agent to "don't ask". Leave it open and you find out what your agents did in the incident review.

## How to actually do it

You don't need any particular vendor for this. You need five pieces that work together:

- A **policy** that decides, for each action, whether to allow it, block it or ask. One policy for every agent and every teammate.
- A way to **reach the person** wherever they are: a notification first, a phone call if they miss it, and waiting if nobody answers.
- **Routing**, so each question goes to whoever owns that decision: the person who started the agent, a named teammate, a role, or the customer.
- A **record** of every decision and who made it, that you can still find months later.
- A **kill switch** that stops one agent or all of them.

A quick check for any agent you run today: what is it allowed to do, who gets asked when it wants more, what happens if they don't answer, where is that written down, and how do you stop it?

---

## Questions people ask

**What is human-in-the-loop for AI agents?**
A person approves, denies or redirects an AI agent's action before it happens, for the steps where a mistake would be costly. Low-risk actions run on their own. High-risk ones wait for a yes.

**When should an AI agent ask a human?**
When the action is hard to undo or reaches outside the agent's sandbox: pushing to a shared branch, deleting data, deploying, spending money, contacting customers. Reading files and running tests usually shouldn't need a person. Principles 1 to 3 cover this.

**What should happen if the person misses the approval notification?**
Escalate. After a minute or two, call their phone, explain what the agent wants and take the answer on the call. If they miss the call too, the action keeps waiting. It never goes ahead by default. See principles 6 and 8.

**Should someone be able to approve a risky step on a phone call?**
Yes, if they're the person allowed to decide. The call should name the danger first and read the yes back. A team can still require some steps to be approved at a keyboard, as a rule it writes down. See principle 7.

**Who should approve an AI agent's action in a team?**
The person who started the agent, by default. Steps that belong to someone else, like a production deploy or a payment, go to a named teammate or a role by rule. Everyone should be able to see afterwards who said yes. See principles 9 and 11.

**How do you keep a person in control of Claude Code, Codex or other coding agents?**
Use their permission hooks to send each tool call through one policy, ask a person only about the risky ones, reach that person on their phone, and record every answer. The same rules should apply to every agent you run.

**What about AI agents that act for customers?**
The customer should approve. A refund, an email to their contacts or a change to their account needs their yes, on their phone, with a record of who answered. The company running the agent can't consent for them.

**Do I need a specific tool to follow this?**
No. Build it yourself, wire it together from open-source parts, or use something that does all of it.

---

## How Pushary does it

I build Pushary, and it's one way to do all of this. You don't need it to follow anything above.

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

If one of these is wrong or missing something, open an issue or a pull request and make the case. If it's better than what's here, I'll change it.

## Citing

Use the "Cite this repository" button on GitHub, or: Aadil Ghani, *Human-in-the-Loop for AI Agents*, version 2, November 2026, https://github.com/Pushary/human-in-the-loop-manifesto.

## License

Released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Copy it, quote it, adapt it, translate it. A link back is appreciated.
