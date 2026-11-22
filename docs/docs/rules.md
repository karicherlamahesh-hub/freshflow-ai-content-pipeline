# Brand guide and compliance rules

The writer step follows the brand guide. The checker step tests every draft against the 10 compliance rules. FreshFlow Software is a made-up company, so these rules are written for this demo.

## About FreshFlow Software

FreshFlow makes software that helps food and manufacturing businesses in India manage stock, orders and production. Typical customers are sweet makers, packaged snack companies and distributors.

## Brand guide

| Area | Guide |
|---|---|
| Tone | Friendly, clear and confident. Write like a helpful expert, not a salesperson |
| Sentences | Short and simple. One idea in each sentence |
| Words to use | simple, reliable, save time, track, plan, easy |
| Words to avoid | best in the world, revolutionary, guaranteed, 100%, magic, cheapest |
| Ending | Always end with one clear next step, such as "Book a demo." |

## Compliance rules

| # | Rule | What it means in practice |
|---|---|---|
| 1 | Do not make claims you cannot prove | Use numbers or percentages only if they come from an approved source |
| 2 | Never promise or guarantee results | No "you will save X" or "guaranteed growth" |
| 3 | Any price must carry a disclaimer | Add: "Prices may change. Contact sales for a current quote." |
| 4 | Do not name or compare competitors | No brand names, no "better than" comparisons |
| 5 | Do not say the software makes a business "FSSAI compliant" | Say it "helps track" records instead |
| 6 | Do not use customer names or quotes without approval | Needs written approval first |
| 7 | Do not make health claims about any food product | No "healthy", "good for digestion" or similar claims |
| 8 | Do not include personal details of real people | No names, phone numbers or addresses |
| 9 | A human must review every AI-written piece before it is published | The pipeline never publishes on its own |
| 10 | Every piece must end with a call to action | For example, "Book a demo." |

## How the pipeline uses the rules

1. The **writer** drafts the content using the brand guide and the rules.
2. The **checker** tests the draft against all 10 rules.
3. The result is one of three statuses:
   - **Approved**: no rule is broken. A person still reviews it before publishing (rule 9).
   - **Refused**: the request breaks a rule, for example it asks for a guarantee or a competitor comparison.
   - **Needs Human Review**: the request is vague or the checker is unsure, for example a claim with no source.

## Examples

| Request | Likely result | Rule |
|---|---|---|
| "Say our software is guaranteed to cut costs" | Refused | 2 |
| "Mention our price is Rs 4,999" | Approved only with the price disclaimer | 3 |
| "Say we are better than a named competitor" | Refused | 4 |
| "Say the software makes you FSSAI compliant" | Rewritten to "helps track" records, or refused | 5 |
| "Say the software saves time" with no source | Needs Human Review | 1 |

These examples show how each rule is meant to work. They are not test results.

## Limits

- The checker is an AI model and can miss problems. Rule 9 is the safety net.
- These rules are for a demo company and are not legal advice.
