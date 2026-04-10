# Frequently Asked Questions

---

## About the Approach

### "This is just ChatGPT with a fancy system prompt."

Fair challenge. The underlying model is the same — but that is like saying every database application is just SQL. The value is in the structured thinking layer on top. Personas encode leadership patterns — what to challenge, what data to demand, what cultural nuance to consider. Skills encode repeatable workflows like 1:1 prep or document review. You could get there yourself with ChatGPT, but you would need to re-engineer the prompt every time and lose consistency. This is the difference between a blank terminal and a purpose-built CLI tool.

### "How is this different from what I already do with AI?"

Most people use AI as a search engine or a text generator — "write me an email," "summarize this doc." This framework treats AI as an adversarial advisor. It pushes back on your assumptions, asks for evidence you might not have, and flags risks you did not mention. The output is not text — it is better thinking. If you already use AI that way, fork the repo and compare approaches. Contributions are welcome.

### "Isn't this over-engineering a prompt?"

If you use AI once a week for a quick question, yes. If you use it daily for decisions, 1:1s, and document reviews — you hit the same problems every time: inconsistent quality, lost context, having to re-explain how you want feedback. This is the same reason we have runbooks instead of ad-hoc debugging. The "engineering" pays off when the workflow is repeated.

---

## Trust and Accuracy

### "How can I trust AI feedback on leadership decisions? It doesn't know my team."

It does not — and that is actually the feature, not the bug. It gives you the outsider view: the logic check, the "have you considered this?" that a peer would give you if they had 30 minutes and full context. You still make the decision. The AI does not replace your judgment — it stress-tests it. And you can feed it as much context as you want: team dynamics, org structure, cultural factors. The more context you give, the sharper the pushback.

### "What about hallucinations? What if it gives me confidently wrong advice?"

Same risk as asking a consultant who does not know your org. You would not blindly follow advice from a consulting deck — you filter it through your judgment. Same here. The sparring partner is most valuable when it surfaces a question you had not thought about, not when it gives you "the answer." If the pushback makes you think harder, it worked — even if the specific suggestion is wrong.

### "AI doesn't understand politics and organizational dynamics."

Correct — it will not tell you that your VP has a pet project you should not challenge in the all-hands. But it will tell you that your proposal lacks a stakeholder analysis, that your communication has no clear ask, or that your 1:1 plan avoids the hard conversation. The political layer is yours. The structural and logical layer is where AI adds value. Most of us lose on logic before we even get to politics.

---

## Practicality

### "How much setup time does this actually take?"

Fork the repo. Open it in your IDE. Start a conversation. That is it — maybe 10 minutes. The personas and skills are pre-built. You do not need to configure anything to get started. The fastest way to evaluate it is to try one real scenario: prep your next 1:1 with it and see if the output is better than what you would have done alone.

### "I barely have time for my current work. Why would I add another tool?"

This is not adding work — it is replacing the time you already spend second-guessing decisions alone, rewriting emails three times, or re-reading the same document after a context switch. If your next 1:1 prep takes 15 minutes instead of 30 and the conversation is better, you saved time and got a better outcome. If it does not help, drop it.

---

## Value and Measurement

### "How do you measure ROI on this?"

The same way you measure ROI on a good mentor: you cannot put a number on it, but you know when you have one. Proxy metrics to track: did your 1:1s lead to clearer action items? Did you catch a flawed assumption before it hit a stakeholder meeting? Did you spend less time rebuilding context after a switch? These are time-and-quality signals, not dashboard metrics. For hard numbers, track time spent on decision prep before and after for two weeks.

### "Will it scale beyond one person?"

The core workflows — decision sparring, 1:1 prep, document conversation — address problems universal to engineering leaders. The framework is designed to be forked and customized: you can adjust personas to your leadership style, add domain-specific context, or build new skills for your workflows. Whether it scales depends on whether the patterns resonate with how you work. Try it and find out.

---

## Philosophical Questions

### "Are we outsourcing leadership to AI?"

No more than using a spell checker outsources writing. The AI does not make your decisions, run your 1:1s, or send your emails. It challenges your thinking before you act. The best leaders all have someone — a peer, a mentor, a coach — who gives them honest pushback. Most of us do not have that person available at 8 PM when prepping tomorrow's escalation. This fills that gap.

### "Won't this make everyone's leadership style the same?"

The opposite. The personas challenge you to think harder, but you decide what to do with that. Ten leaders will get the same pushback on a weak 1:1 plan — and then do ten different things with it based on their team, their style, and their context. It is like a gym: everyone uses the same equipment, nobody gets the same body.

### "What happens when the AI disagrees with my gut?"

Always follow your judgment. But if the AI raises a point and you cannot articulate why your gut says otherwise — that is the signal to dig deeper. The most dangerous decisions are the ones where you cannot explain your reasoning. The AI forces you to articulate it. That alone is worth the exercise.

---

## Technical

### "Will meeting transcript analysis work on a real chaotic call with 12 people?"

It handles messy better than clean, actually. The value is in spotting what was not decided, what was avoided, and where the conversation went circular. On a clean meeting with clear outcomes, you do not need the tool. On a chaotic 12-person call where you left wondering "what did we actually decide?" — that is where it earns its keep.

### "The 1:1 skill uses SBI feedback format. What if I prefer a different framework?"

Fork it and change it. The skill is a Markdown file. Replace SBI with COIN, radical candor, or whatever model you use. The point is having a structured format that forces specificity — not the specific framework. If you build a better version, contribute it back.

### "Why not just use Rovo or Copilot for document review?"

Rovo and Copilot are Q&A tools that retrieve and summarize content from a document store. Useful, but different job. The doc assistant here does three things they do not: First, it walks you through a document section by section at your pace — designed for context-switching between meetings, not one-shot retrieval. Second, it stays scoped strictly to the document unless you explicitly ask for broader context, so you know exactly where the information comes from. Third, it works on any format — Word, PDF, Markdown — not locked to one platform. And once you are in a doc conversation and spot a suspicious claim, you can switch to due diligence for fact-checking. Need to make a decision based on what you read? Switch to the engineering-leader sparring partner. Platform-native tools stop at the answer. This framework composes.

### "The document conversation mode seems slow compared to just reading the document."

It is slower than speed-reading, yes. But it is faster than reading, losing context, re-reading, and then realizing you missed the key trade-off buried in section 4. The value is retention and depth, not speed. For a 3-page doc, just read it. For a 30-page architecture proposal you need to comment on intelligently across three meetings this week — the conversation mode saves you the re-read tax every time.

---

## Contributing and Vision

### "What is the long-term vision?"

A growing collection of leadership personas and skills that get smarter over time, maintained by practitioners who use them daily. The roadmap includes integrations with collaboration tools (Slack, Jira) to reduce manual input, and community-contributed personas and skills for domains beyond engineering leadership. The framework gets better with every person who uses it critically and shares what they learn.

### "Can I contribute a persona or skill?"

Yes — that is the point. If you have a workflow that could benefit other leaders, write it as a skill and submit a PR. If you think a persona is missing something based on your experience, open an issue. See the repo's contributing guidelines for details.
