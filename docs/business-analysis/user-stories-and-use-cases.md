# User Stories & Use Cases

The final phase: translating strategy into a real content backlog, checking whether those stories actually hold up, and formalising the pipeline at a level of precision suitable for handing to a developer.

## SIPOC: A One-Page View First

Before diving into individual stories, a SIPOC diagram (Suppliers, Inputs, Process, Outputs, Customers) gave a one-page summary of the whole pipeline, useful for a pitch deck or onboarding pack in a way a six-step process document isn't.

| Suppliers | Inputs | Process | Outputs | Customers |
|---|---|---|---|---|
| Co-founders (topic direction) | Approved topic brief | 1. Select topic | Published, validated article | Expat readers |
| Regulatory / tax sources | Source-of-truth data | 2. AI drafts article | Updated topic briefs (re-queued) | Spanish citizen readers |
| Product providers (reference only) | Prompt library & style guide | 3. Review & fact-check | Logged validation records | Lirafin's own brand & reputation |
| Readers (feedback) | Reader feedback signals | 4. Founder sign-off, 5. Publish, 6. Monitor | | |

## Content Backlog

Lirafin's seven content categories, Money, Deals, Property, Tax, Living, Work, Travel, became 21 example user stories, prioritised with MoSCoW:

| Category | Story | Priority |
|---|---|---|
| Tax | As an expat who recently became a Spanish tax resident, I want to understand my Modelo 720 obligations, so that I don't get fined for undeclared foreign assets | Must have |
| Money | As a reader comparing digital banks, I want an unbiased comparison of fees and features, so that I can pick one without worrying about hidden sponsorship bias | Must have |
| Living | As a new resident, I want a plain-language explanation of NIE vs. TIE, so that I know which one I actually need | Must have |
| Property | As a non-resident buyer, I want a comparison of mortgage options available to me, so that I can find viable financing | Should have |
| Deals | As a budget-conscious reader, I want a running list of the best current bank switching bonuses | Won't have (for now) |

The pattern was telling: Must-have stories clustered around Tax, Money, and Living, the categories where getting something wrong has real financial or legal consequences. Deals content mostly landed as a deliberate Won't-have, correctly reflecting that it depends on operational capacity the business isn't resourced for yet.

## INVEST Quality Check

Writing a story in the right format doesn't make it a good story. Running all 21 against INVEST (Independent, Negotiable, Valuable, Estimable, Small, Testable) caught real issues before they reached drafting:

| # | Story | I | N | V | E | S | T | Notes |
|---|---|---|---|---|---|---|---|---|
| 4 | Unbiased digital bank comparison | Y | Y | Y | N | N | Y | Too large as written; split into a comparison table plus per-bank deep dives |
| 8 | Home insurance comparison | Y | Y | N | N | N | Y | Not estimable without a bounded provider shortlist first |
| 19 | Bank switching bonuses list | Y | N | Y | N | Y | N | Not independent (depends on a live data feed) or testable as written |

11 of 21 stories passed cleanly. 10 had at least one flagged issue, mostly "comparison" stories that read fine on paper but weren't actually estimable without a defined provider list. Catching that on paper is considerably cheaper than catching it after a wasted drafting cycle, and the two Deals stories that failed confirmed that marking them Won't-have in the first place had been the correct call, not just a guess that happened to land well.

## Use Case Diagram

The swimlane diagram is the right tool for explaining the process to a founder. It's the wrong tool for briefing a developer building actual workflow tooling. So the pipeline was also formalised as a UML use case diagram, with actors, use cases, and explicit include/extend relationships:

```
    Founder                    ┌─────────────────────────────────┐                 AI System
       │                       │      Lirafin Content Pipeline     │                    │
       ├──────────────────────►│         (Select Topic)            │                    │
       │                       ├───────────────────────────────────┤                    │
       │                       │         Generate Draft            │◄───────────────────┤
       │                       │              ▲ «extend»            │                    │
       │                       │      Flag Unverified Claims       │◄───────────────────┤
       │                       ├───────────────────────────────────┤                    │
       │                       │         Validate Draft            │            Human Reviewer
       │                       │              ▲ «include»           │                    │
       │                       │         Apply Checklist           │◄───────────────────┤
       │                       ├───────────────────────────────────┤                    │
       ├──────────────────────►│        (Approve Publish)          │                    │
       │                       ├───────────────────────────────────┤                    │
       │                       │      Detect Regulatory Change     │◄───────────────────┘
    Reader                     └─────────────────────────────────┘
       │  "feedback informs"            ↻ triggers a new
       └────────────────────►      Select Topic instance
```

**Relationships worth noting:** Flag Unverified Claims *extends* Generate Draft, it only runs when the AI system can't verify a claim, an optional extension rather than a mandatory step. Apply Checklist is *included by* Validate Draft, every validation always applies the checklist, a mandatory sub-step, not optional.

**Worked exception flow, Validate Draft:** if one or more checklist items fail substantively (a factual claim can't be confirmed, or the piece strays into regulated advice), the reviewer returns the draft to the AI system with specific revision notes, the fail-review loop visible in the swimlane diagram, and the use case ends without publication.

## Reflection

Across all four phases of this engagement, the work touched five of BABOK's six knowledge areas: elicitation and collaboration, strategy analysis, requirements analysis and design definition, requirements life cycle management, and stakeholder analysis. The one still open is Solution Evaluation, the success metrics that would confirm any of this works once Lirafin runs its first real batch of content through the pipeline. That's the natural next phase.

The recurring lesson across the whole engagement: the diagrams and registers are the visible output, but the real value came from the questions they forced into the open, questions that had been living, unexamined, in the founders' heads until someone wrote them down.

---

*This case study describes consulting work for an early-stage company. Some details are generalised to respect the company's confidentiality.*
