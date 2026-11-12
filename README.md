# Wake the Human

**When should an always-on ops agent page a human?** An inverse-CDF Monte Carlo prototype that picks how many failed attempts to wait before paging on-call.

- **Live demo:** https://shriyashekatkar25-byte.github.io/wake-the-human/
- **Gallery:** https://shriyashekatkar25-byte.github.io/apm-lab/
- Part of **APM Lab** by Shriya Shekatkar (UCLA, Stats & Data Science minor) · Stats 102C Monte Carlo Methods applied to product decisions.

## The launch that inspired it
On **Nov 12, 2026**, Databricks release notes listed **Genie ZeroOps** as available in Beta: a background agent that watches jobs, pipelines and tables, flags failures, traces root causes and suggests fixes.
- Release notes (Nov 2026): https://docs.databricks.com/aws/en/release-notes/product/2026/november
- Product docs: https://docs.databricks.com/aws/en/zeroops/
- Launch blog (June 2026): https://www.databricks.com/blog/introducing-genie-zeroops

According to the docs, findings appear as "insights" in an inbox, and users can set up notifications for new ones. That raises the product question below.

## The PM decision
**Default paging rule:** page the on-call engineer after **N consecutive failed attempts** of a job. There is also an optional **fast lane** that pages on the first failure when the agent's diagnosis is deterministic (schema change, bad code push), since retrying can't fix those.

| | |
|---|---|
| Success metric | Median time-to-human on real breaks ≤ 45 min |
| Guardrails | False pages ≤ 2.0 per on-call week · ≥ 95% of real breaks paged within 60 min · ≤ 10% noisy weeks (more than 10 pages) · ≤ 0.75 false pages per week between 10 p.m. and 7 a.m. |
| Stakeholders | On-call data engineers, analytics leads, agent research team, jobs/pipelines PMs, field engineering |

With the default scenario (12 workspaces, about 29 findings a week, 15% real breaks, seed 1102), the memo recommends **N = 4 with the fast lane on**. Move the sliders and the memo rewrites itself.

## What it shows about product judgment
- **Customer problem framing:** the real customer is the on-call engineer who will mute a noisy agent, not the agent itself.
- **Prototyping:** a working, seeded simulator instead of a slide deck.
- **SQL/Python:** sketches of how to fit the inputs from job-run history and replay the policy in shadow mode.
- **Data-driven decisions:** guardrails come first, and the recommendation is the fastest N that clears all of them.
- **Clear writing:** a director memo with a one-sentence version for the VP.

## Methods
| Course idea (Stats 102C) | Where it's used |
|---|---|
| Ch3 Inverse CDF, exponential | Gaps between findings and attempt runtimes: `−ln(1−U)/λ` |
| Ch3 Inverse CDF, geometric | Failures before a flaky job heals: `⌊ln(1−U)/ln(1−p)⌋` |
| Ch1 Monte Carlo expectations | 2,000 simulated weeks estimate noisy-week share, night pages and on-time paging |
| Exact check | Expected false pages = findings × (1−q) × (1−p)^N, shown beside the estimate |

## Design baseline
All visuals are original SVG, CSS and canvas drawings. No pin images are used.
- [“MONEY IS COMING” by KAPOSTELOUN](https://www.pinterest.com/pin/1144336586610336771/): hero. Forest green (#2f5a07) with a butter-yellow (#f4f1a6) heavy grotesk headline.
- [“2026 manifesting” by slaywithsona](https://www.pinterest.com/pin/982488474976370748/): cards, buttons and memo. Kraft paper, red handwritten ink (#a8261e), small bows and underline squiggles.
- [Iridescent glitter-hands painting by ৻ꪆ](https://www.pinterest.com/pin/921689879977602179/): KPI badges. Lilac, blush, gold and aqua iridescent gradient with sparkle stars.
- [“kiana's page” (no mud no lotus) by kiana](https://www.pinterest.com/pin/898960775641848480/): simulation room. Dark grainy pond green, pink lotus marks, cream condensed caps.

## Related work
- Same area: [leash-length](https://github.com/shriyashekatkar25-byte/leash-length) (agent time limits, inverse CDF), [runaway-tab](https://shriyashekatkar25-byte.github.io/runaway-tab/), [fresh-enough](https://github.com/shriyashekatkar25-byte/fresh-enough), [claim-check-clock](https://github.com/shriyashekatkar25-byte/claim-check-clock)
- Method notes: [say-cheese-monte-carlo](https://github.com/shriyashekatkar25-byte/say-cheese-monte-carlo) (Ch1–3), [roll-the-dice-math](https://github.com/shriyashekatkar25-byte/roll-the-dice-math) (Ch1)
- Everything else: [APM Lab gallery](https://shriyashekatkar25-byte.github.io/apm-lab/) · [GitHub profile](https://github.com/shriyashekatkar25-byte)

---
Unofficial concept prototype by Shriya Shekatkar. All data is simulated. Not affiliated with any company.
