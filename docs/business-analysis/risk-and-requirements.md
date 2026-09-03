# Risk & Requirements

The third phase: moving past surface-level risk statements to root causes, and making sure nothing important about the business was being quietly assumed rather than actually stated.

## Root Cause, Not Just Symptoms

The highest-rated risk in the process was human review capacity becoming a bottleneck as AI-generated content volume increases. It's an easy risk to write down and move past. Rather than leave it there, I ran a Five Whys analysis on it.

| Level | Question and answer |
|---|---|
| Why 1 | Why might human review become a bottleneck? Because AI produces drafts far faster than a human can validate them |
| Why 2 | Why does validation take so much longer than generation? Because it requires fact-checking, regulatory-currency checks, tone checks, and bias checks, all manual, with no automated support today |
| Why 3 | Why is there no automated support for these checks? Because no tooling has been built to pre-check facts before a draft reaches the reviewer |
| Why 4 | Why hasn't that tooling been built yet? Because the business prioritised designing the process before investing in tooling for a step that hadn't been tested at real volume |
| Why 5 | Why hasn't the process been tested at volume yet? Because no batch of content has actually been piloted through the pipeline end to end |

**Root cause:** the bottleneck is structural, not a resourcing failure. It's the natural consequence of automating content generation before automating validation support, at a stage where the business hadn't yet piloted the pipeline to learn how much support was actually needed. That distinction changes the fix entirely, from "hire someone" to "pilot first, then build the right tooling."

## Risk Register

That analysis fed into a consolidated risk register, pulling together everything that had been scattered as individual notes across the stakeholder register and process documentation:

| Risk | Likelihood | Impact | Rating | Mitigation owner |
|---|---|---|---|---|
| Human review capacity becomes a bottleneck | Medium-High | High | High | Co-founders |
| AI hallucinated or stale content reaches publication unchecked | Medium | High | High | Human reviewer |
| Outdated regulatory info not caught before publish | Medium | High | High | Co-founders |
| Undisclosed commercial bias affects recommendations | Low | High | Medium-High | Co-founders |
| Founder sign-off becomes a bottleneck as volume scales | Medium | Medium | Medium | Co-founders |

*(Full register runs to ten risks; the five above are the highest-rated.)*

## Requirements Classification

Lirafin had a mission statement, a stakeholder register, and a process document, but nothing had ever formally classified what the business actually required, by type. Running a classification across BABOK's six standard categories surfaced two requirements that had been implicit the entire time without ever being written down.

| Type | Example requirement | Notes |
|---|---|---|
| Business | Lirafin shall become the recognised independent authority on personal finance in Spain | From the mission statement |
| Stakeholder | Expat readers require guidance on Spanish systems in plain, accessible language | From the stakeholder register |
| Functional | No AI-drafted content shall publish without a named human reviewer sign-off | From the governance notes |
| Non-functional | Site content should be written at a reading level accessible to non-native Spanish speakers | **Previously implicit, never stated until this pass** |
| Transition | Human review capacity must be assessed before content volume increases materially | From the risk register |
| Regulatory | Any future referral relationship with a product provider must be disclosed | **Previously implicit, never stated until this pass** |

## Process Automation Assessment

Lirafin's mission commits to over 80% AI automation. That number is easy to chase opportunistically, automating whatever's easiest rather than whatever matters most. So each of the six pipeline steps was ranked deliberately:

| Pipeline step | Automation potential | Priority | Rationale |
|---|---|---|---|
| Select topic | Medium | Medium | AI can flag regulatory triggers, but the judgement call should stay human |
| Generate draft | Already automated | n/a | Core AI function |
| Review & fact-check | Medium (assist, not replace) | High | Highest-value candidate: pre-checks reduce workload without removing the human gate |
| Founder sign-off | Low | Low | Automation risks undermining the accountability the step exists to provide |
| Publish | High | Medium | Scheduling and metadata are mechanical tasks well suited to automation |
| Monitor for changes | High | High | Tracking regulatory changes at scale is impractical manually |

The ranking wasn't what I expected going in. The steps that guard accountability (sign-off, topic selection) scored *low* on automation priority, not because they're hard to automate, but because automating them would undermine the reason they exist. The review step landed in the middle: not full automation, but AI-assisted pre-checks that reduce workload without removing the human validation gate, directly addressing the bottleneck the Five Whys analysis traced back to a structural gap.

---

Next: [User Stories & Use Cases](user-stories-and-use-cases.md), turning the content strategy into stories, then checking whether those stories actually hold up.
