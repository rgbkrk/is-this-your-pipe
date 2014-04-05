Is this your pipe?
==================

# Hijacking the Build Pipeline

These are the talk materials for a submitted talk on credential and infrastructure hijacking from public repositories.

## Avenues of attack

### Search for "cloud" credentials

*Tier 1 (passive)*

Use GitHub's search API to find repositories that leave credentials out in the wide open.

Release `gitsecnanny` to help combat against Tier 1 for the public at large.

### Submitting accidental PRs

*Tier 2 (active, masquerade as oops)*

Submit a PR against a repo that uses "encrypted" components on Travis CI, send them to a webhook or just echo them out.

User sets up CD with Heroku:

```bash
travis encrypt $(heroku auth:token) --add deploy.api_key
```

```yaml
deploy:
  provider: heroku
```

> Please note that secure env variables are not available for pull requests from forks. This is done due to the security risk of exposing such information in submitted code. Everyone can submit a pull request and if an unencrypted variable is available there, it could be easily displayed.

Code review stands. Make sure no one does anything stupid with your "secure"
environment variables before merging code in.

> Please also note that keys used for encryption and decryption are tied to the repository. If you fork a project and add it to travis, it will have different pair of keys than the original.

### Submitting purposeful infrastructure modifications

*Tier 3 (active, aggressive)*

Submit a PR against a repo that uses Jenkins as part of a CI/CD workflow.

## Moral of the story:

*Don't assume trust with your build pipeline.*

However, don't let this limit your ability to keep deploying good products. I really hate it when security minded folks go straight to tightening up their sphincter instead of letting goods get delivered.

I promise more vulgarity in the DefCon talk.
