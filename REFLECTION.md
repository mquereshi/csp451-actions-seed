# CK3 Reflection

This checkpoint made clear the difference between CI/CD as a concept and CI/CD as
something that actually blocks bad code from reaching `main`. Wiring up the
`/health` endpoint was simple; the real lesson was watching the pipeline enforce
quality automatically. The coverage threshold in `package.json` meant untested code
would fail the build outright, and the `npm audit` gate caught real vulnerabilities
in nested dependencies (`qs`, `js-yaml`, `form-data`) pulled in transitively through
`express` and dev tooling, nothing to do with code I wrote. `npm audit fix` resolved
them without touching application logic, which showed how much "security" at this
level is dependency hygiene rather than application code.

The part I got stuck on was Dependabot: committing `.github/dependabot.yml` wasn't
enough on its own, the feature had to be explicitly enabled under Settings →
Advanced Security (alerts, security updates, and version updates each toggled
separately) before the Dependabot dashboard showed anything. It reinforced that
GitHub's automation is often split between config-as-code and repo settings, and
both halves need to be in place. The red-then-green debug cycle drove home how much
faster a PR check catches a formatting mistake than manual inspection would.
