Is this your pipe?
==================

# Hijacking the Build Pipeline

These are the talk materials for a submitted talk on credential and infrastructure hijacking from public repositories.

## Avenues of attack

### Search for "cloud" credentials

*Tier 1 (passive)*

Release `gitsecnanny` to help combat against Tier 1 for the public at large.

### Submitting accidental PRs

*Tier 2 (active, masquerade as oops)*

Submit a PR against a repo that uses "encrypted" components on Travis CI, send them to a webhook or just echo them out.

### Submitting purposeful infrastructure modifications

*Tier 3 (active, aggressive)*

Submit a PR against a repo that uses Jenkins as part of a CI/CD workflow.

## Moral of the story:

*Don't assume trust with your build pipeline.*

However, don't let this limit your ability to keep deploying good products. I really hate it when security minded folks go straight to tightening up their sphincter instead of letting goods get delivered.

I promise more vulgarity in the DefCon talk.
