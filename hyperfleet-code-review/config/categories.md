# Finding categories (ordered by priority)

Assign one of these categories to each finding. When deduplicating findings from multiple
sources, use the highest-priority category.

1. **Bug** — Bugs and logic issues
2. **Security** — Security issues
3. **Architecture** — Inconsistencies with HyperFleet architecture docs
4. **JIRA** — Does not meet JIRA ticket requirements (review-pr only)
5. **Standards** — Deviations from HyperFleet coding standards
6. **Inconsistency** — Internal inconsistencies and contradictions
7. **Deprecated** — Outdated or deprecated versions
8. **Pattern** — Project patterns not followed
9. **Improvement** — Clarity and maintainability improvements

## Default severity by category

| Category | Default severity | Override when |
|----------|-----------------|---------------|
| Bug | Blocking | nit if cosmetic or edge-case-only with no user impact |
| Security | Blocking | nit if theoretical/defense-in-depth only |
| Architecture | Blocking | nit if minor style deviation with no structural impact |
| JIRA | Blocking | nit if the gap is a nice-to-have beyond acceptance criteria |
| Standards | Blocking | nit if the standard explicitly marks the rule as optional/recommended |
| Inconsistency | Blocking | nit if both approaches are acceptable and the inconsistency is within a single file |
| Deprecated | Blocking | nit if the deprecation has no timeline or the replacement is not yet stable |
| Pattern | nit | Blocking if ignoring the pattern causes concrete bugs or breaks tooling |
| Improvement | nit | Blocking only if readability is so poor it hides a real bug |

Use the default unless the specific finding clearly matches an override condition. When
overriding, the "Problem" section should briefly explain why the severity differs from the
default.

## Confidence classification

Every finding carries a confidence level indicating how certain the analysis is that the
finding is a real problem (as opposed to a false positive):

- **High** — strong evidence directly visible in the diff; the problem is almost certainly real
- **Medium** — probable issue, but depends on context not fully visible in the diff
- **Low** — possible concern that the reviewer should verify; may be a false positive

| Signal | Confidence |
|--------|------------|
| Bug is syntactically provable (nil deref, missing return, wrong type) | High |
| Pattern violates a fetched HyperFleet standard with an exact rule match | High |
| JIRA acceptance criterion is clearly unmet | High |
| Issue depends on runtime behavior or external state | Medium |
| Code looks suspicious but may be intentional (e.g., empty error handler with a comment) | Medium |
| Style/naming suggestion based on convention rather than a rule | Low |
| Issue found by analogy ("other places do X, so this should too") without a standard backing it | Low |

When confidence is **Low**, the "Problem" section should explain what would confirm or rule
out the issue.
