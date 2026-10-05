# Remediation plan: Karim Aziz after PA1 (2026-12-08)

## Diagnosis

| Item | Objectives | Chosen | Correct | Points | Cause | Detail | Flags |
| --- | --- | --- | --- | --- | --- | --- | --- |
| PA-09 | U-15 | C | D | 0/1 | habit | wrong-detail | — |
| PA-13 | U-02 | C | A | 0/1 | habit | wrong-detail | changed from right |
| PA-14 | E-17 | C | B | 0/1 | confusion | async-plugin-step~background-operation | looked up |
| PA-21 | E-26 | C | B | 0/1 | confusion | api-permission~security-role | confident |
| PA-22 | B-13 | B | A | 0/1 | confusion | agent-flow~cloud-flow | — |
| PA-23 | U-18 | A, E | A, B | 1/2 | confusion | csp-frame-ancestors~csp-frame-src | — |
| PA-32 | I-13 | A | D | 0/1 | confusion | dynamic-values~openapi-enum | — |
| PA-38 | E-15 | A | B | 0/1 | habit | wrong-detail | confident |
| PA-42 | B-17 | C | B | 0/1 | confusion | appinsights-export~log-search-alert | confident |

## Findings

- Behind plan at the last checkpoint: +3 min at PA-43; worst +5.1 at PA-20
- More Learn minutes than the budget: 9 min on PA-14, PA-19 (budget 6); 1 right afterwards
- A right answer changed to a wrong one: right to wrong: PA-13
- A domain 10 or more points below its C-73 readiness: build 78% vs 89%

## Labs (275 of 300 minutes, 2026-12-09 to 2026-12-10)

| Day | Lab | Minutes | Covers |
| --- | --- | --- | --- |
| 2026-12-09 | R1 Pace, Learn budget and answer changes | 30 | finding:pace, finding:learn-budget, finding:second-guessing |
| 2026-12-09 | R8 Choosing the automation host | 20 | objective:B-13, pair:agent-flow~cloud-flow, habit:wrong-place |
| 2026-12-09 | R5 Lookup filters and CSP directives | 35 | objective:U-02, objective:U-18, pair:csp-frame-ancestors~csp-frame-src, habit:wrong-detail |
| 2026-12-09 | R7 Code app flows and connector metadata | 35 | objective:U-15, objective:I-13, pair:dynamic-values~openapi-enum |
| 2026-12-09 | R3 Identity gates: token, user, role, resource | 30 | objective:E-26, pair:api-permission~security-role |
| 2026-12-10 | R4 Alert latency budget | 30 | objective:B-17, pair:appinsights-export~log-search-alert |
| 2026-12-10 | R6 Long-running custom API calls | 25 | objective:E-17, pair:async-plugin-step~background-operation |
| 2026-12-10 | R2 Bulk writes that isolate bad rows | 35 | objective:E-15 |
| 2026-12-10 | R9 Calibration: re-rate Build closed-book | 35 | finding:calibration |

Every target is covered by a scheduled lab.

Re-test on the next form: B-13, B-17, E-15, E-17, E-26, I-13, U-02, U-15, U-18.
