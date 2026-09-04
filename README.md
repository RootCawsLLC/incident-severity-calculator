# Incident Severity Calculator

Rate a security finding or incident against the **OWASP Risk Rating Methodology** — sixteen factors,
likelihood and impact kept separate, and one rubric every team derives their answer from.

**Live:** https://rootcawsllc.github.io/incident-severity-calculator/

![The calculator on the risk-lab shell. A full-width introduction under the lab kicker, then the four factor groups as cards with Fraunces headings, the impact-leg choice, three score tiles, a Critical severity banner read from the OWASP matrix, the matrix with the active cell highlighted, the copyable rating record, and three closing notes in cards](preview.png)

## Why this exists

Ask three engineers to rate the same finding and you get three answers. The disagreement is rarely
about the finding — it is about the absence of a shared scale. Everyone is rating against a private
sense of what "high" means, and none of those senses are written down.

OWASP's methodology sidesteps the problem by never asking the question directly. Instead of *how bad
is this*, it asks sixteen narrower questions with pre-scored answers — how skilled must the attacker
be, how large is the population who could try, is this logged, is it public knowledge — and derives
severity from the arithmetic.

The payoff is not accuracy. It is that the argument moves from "I think this is high" to "you scored
intrusion detection at 9 and I scored it at 3" — a question that has an answer someone can go check.

## What it does

Sixteen factors in four groups, each option carrying the score OWASP assigns it:

| Group | Leg | Factors |
| --- | --- | --- |
| Threat agent | Likelihood | Skill level, motive, opportunity, population size |
| Vulnerability | Likelihood | Ease of discovery, ease of exploit, awareness, intrusion detection |
| Technical impact | Impact | Loss of confidentiality, integrity, availability, accountability |
| Business impact | Impact | Financial damage, reputation damage, non-compliance, privacy violation |

Likelihood is the mean of the eight threat-agent and vulnerability factors. Impact is the mean of
either impact group — you choose which leg drives the outcome, because OWASP is explicit that
business impact should win whenever you can estimate it credibly. A technically severe finding on a
system nobody depends on is not a severe business problem.

Each leg lands in a band (`<3` low, `3–6` medium, `≥6` high), and overall severity is read from the
OWASP matrix:

| Impact ↓ / Likelihood → | LOW | MEDIUM | HIGH |
| --- | --- | --- | --- |
| **HIGH** | High | Critical | Critical |
| **MEDIUM** | Medium | Medium | High |
| **LOW** | Note | Low | Medium |

The active cell is highlighted live as you rate, so the derivation stays visible rather than
disappearing into a number. A copyable rating record captures every factor selection alongside the
result.

### Calibrating "Financial damage"

OWASP's financial-damage factor asks you to choose between *minor effect on annual profit* and
*significant effect on annual profit* without ever saying what either means. Enter an annual profit
figure and pick a comparable published loss range, and the panel shows what a typical event, a bad
event and the floor actually cost — each as a share of the profit you entered, with every source and
its stated limitation behind it.

It does not pick an option for you, and it cannot change your score. Reading one number off a
distribution means choosing a percentile, which is the rater's judgment and belongs in the record as
such. The panel's job is to stop the choice being made in the dark.

## Honest limits

**Averaging ordinal scores is not sound measurement.** "Partners" scoring 5 and "authenticated users"
scoring 6 does not make the second 20% more of something. Treat the decimals as a sorting aid, never
as a quantity, and never as an input to a financial model. If you want defensible numbers, that is
what [FAIR](https://github.com/RootCawsLLC/fair-model-study) is for.

**Severity is not materiality.** A critical finding may be immaterial to a filer, and a medium one
touching regulated data may not be. They are different questions, asked by different people, with
different consequences for getting them wrong. For the second, see the
[Cyber Materiality Workbench](https://github.com/RootCawsLLC/cyber-materiality-workbench), which picks up
where this leaves off.

**A rubric is a floor, not a ceiling.** It makes ratings comparable and arguments specific. It does
not make them right, and it should never stop someone escalating a finding the matrix scored low.

**The calibration panel describes a population, not your finding.** A benchmark says what breaches
cost across a set of organisations; it says nothing about the specific weakness in front of you, and
a rater who treats it as a prediction has substituted one unfounded number for another. It is also
priced in its own currency, with no conversion applied — the profit figure and the shard have to
agree, and the panel names the shard's currency so you can check. Where a shard has no practitioner
statement of what it is not good for, that absence is a gap in the record rather than a clean bill
of health.

## Running locally

Single self-contained `index.html` — React 18 via UMD CDN, no build step, no dependencies.

```bash
python -m http.server 8000
```

Then open http://localhost:8000. Nothing you enter leaves the browser — no backend, no storage, no
telemetry. The page fetches React, the webfonts, and the loss-benchmark corpus used by the
calibration panel; none of those requests carries anything you entered, and if the corpus is
unreachable the panel says so and the rating works unchanged.

## Attribution

Factor definitions and their scores follow the
[OWASP Risk Rating Methodology](https://owasp.org/www-community/OWASP_Risk_Rating_Methodology),
published by the OWASP Foundation under CC BY-SA.

Calibration data comes from [risk-benchmarks](https://github.com/RootCawsLLC/risk-benchmarks), which
derives it from [RiskShard](https://github.com/raviaxo/RiskShard) by
[raviaxo](https://github.com/raviaxo), AGPL-3.0. Ranges, confidence levels and limitations are
RiskShard's, carried through unchanged.

## License

Copyright (c) 2026 RootCaws LLC.

[GNU AGPL v3 or later](LICENSE). If you modify this and run it as a network service, the AGPL requires you to offer your users the modified source under the same terms.
