# Process Modeling

The second phase: turning Lirafin's content pipeline, a blend of AI drafting and human validation, into an actual documented process rather than something that lived only in the founders' heads.

## Method

A plain flowchart can show the sequence of steps, but it can't show who owns each one, and ownership is exactly where a process breaks down at scale. So the pipeline was modeled as a swimlane (cross-functional) diagram, one lane per actor, following the process modeling techniques covered in BABOK's Requirements Analysis and Design Definition area.

## Swimlane Diagram

```
FOUNDER                AI SYSTEM              HUMAN REVIEWER         PUBLISH & MONITOR
────────────────────────────────────────────────────────────────────────────────────
Select topic
(from backlog)
    │
    └──────────────────► AI drafts
                          (from brief)
                              │
                              └──────────────────► Review draft
                                                    (fact-check)
                                                        │
                              ◄────── "Fails: back to AI" ┘
                                                        │ Pass
Sign-off  ◄─────────────────────────────────────────────┘
(founder OK)
    │
    └──────────────────────────────────────────────────────────────► Publish
                                                                          │
                                                                          ▼
                                                                     Monitor
                                                                (tracks changes)
                                                                          │
                                                          ↻ Re-queues topic (back to Select topic)
```

Two feedback loops keep the pipeline self-correcting rather than linear: a draft that fails validation returns to the AI system for revision, and a published article flagged during monitoring re-enters the backlog as a new topic brief.

## Process Detail

Each of the six steps was specified with its owner, purpose, inputs, key activities, outputs, tools, and risks. The step that turned out to matter most:

| Field | Detail |
|---|---|
| Owner | Human reviewer / validator |
| Purpose | Ensure the draft is accurate, current, on-brand, and independent before it can proceed to founder sign-off, the pipeline's hard quality gate |
| Inputs | Draft article; AI's unverified-claims list; validation checklist |
| Key activities | Verify facts against primary sources; confirm no outdated regulatory or product information; check brand tone; confirm no undisclosed commercial bias |
| Outputs | Either an approved draft passed to sign-off, or the draft returned to the AI system with revision notes |
| Tools & techniques | Validation checklist; primary-source cross-referencing |
| Risks & notes | Reviewer capacity is the most likely bottleneck as AI output volume grows |

## RACI Matrix

Once the process existed on paper, assigning Responsible, Accountable, Consulted, and Informed to each step made ownership impossible to leave vague:

| Pipeline step | Co-founders | AI system | Human reviewer | BA Consultant |
|---|---|---|---|---|
| Select topic | R / A | C (flags triggers) | I | C |
| Generate draft | I | R | I | I |
| Review & fact-check | I | I | R / A | I |
| Founder sign-off | R / A | I | C | I |
| Publish | A | I | I | I |
| Monitor for changes | A | I (surfaces alerts) | C | I |

## What This Surfaced

The RACI matrix made an uncomfortable but useful fact explicit: the co-founders currently hold both Responsible and Accountable on two separate steps, topic selection and sign-off. That concentration is entirely reasonable at the current team size, but it's a scaling risk worth watching rather than an accident worth ignoring. It also raised an open governance question that had no obvious answer yet: should founder sign-off apply to every single article, or be tiered by content risk level, with a simple deals roundup treated differently from a tax-obligation piece?

---

Next: [Risk & Requirements](risk-and-requirements.md), what happened when I traced the pipeline's biggest flagged risk back to its actual root cause.
