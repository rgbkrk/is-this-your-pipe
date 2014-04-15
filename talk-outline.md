Talk Outline
------------

We'll cover three tiers of hijacking infrastructure using components of the build pipelines. We'll show how we help secure others and how you can secure your build pipeline too.

### SecOops: Credentials in public repositories

In the rush to deploy code and get services moving quickly in collaboration, coders will put credentials in public facing repositories.

With a quick search, you too no longer need your own money to mine DogeCoin. GitHub makes it really easy through their code search API to find the most recently indexed results.

[Recent AWS keys uploaded](https://github.com/search?o=desc&q=AKIAJ&ref=searchresults&s=indexed&type=Code) (`AKIAJ`)

> I did not completely scrub my code before posting to GitHub. I did not
have billing alerts enabled ... This was a real mistake ... I paid the
price for complacency

Source: https://securosis.com/blog/my-500-cloud-security-screwup

#### Sound the Alarm! Alert the World!

To combat this for the public at large, we're releasing gitsecnanny.

GitSecNanny searches repositories for security oops. The first we're going after is credentials left out in the open in public repositories.

After finding credentials, we email the original committer and the owner of the project to let them know how to revoke keys.

Anonymized examples and responses will be shown.

### Hijacking encrypted secrets

The next tier is submitting a PR against a repository that uses encrypted secrets on Travis CI. Pulling them out can be as simple as writing them out to std(out|err) or even as complex as posting to a webhook.

Travis mitigates this issue by not making the secure env variables available on pull requests from forks.

> Please note that secure env variables are not available for pull requests from forks. This is done due to the security risk of exposing such information in submitted code. Everyone can submit a pull request and if an unencrypted variable is available there, it could be easily displayed.

Props to them for making sure to think about this.

What about forking the repository and adding it to Travis itself? The encryption keys are tied to the repository.

> Please also note that keys used for encryption and decryption are tied to the repository. If you fork a project and add it to travis, it will have different pair of keys than the original.

Is there anything to be worried about then?

Yes. Code review still stands. Make sure no one does anything stupid with your "secure" environment variables before merging code in.

Masquerade process:

1. Find repository with credentials
2. Do legitimate work on a feature or bug
3. Include your security oops
...
4. Profit

Downside: These attribute "you" to making these pull requests though.
Upside: You're not really "you" though, are you?

### Hijack the build, infect staging+production

Jenkins gives you a lot more build power than Travis does. You have more control over the builds, what gets installed, what hooks you have, and what is available to you.

People use Jenkins to do continuous deployment from dev -> staging -> production. That's great! Yay for features!

What happens when you trust your build pipeline too much, along with the world?

#### Get Your Own Butler!

![](https://cacoo.com/store/stencil/image?id=272331&storeItemVersionId=10223)


Do another nasty PR that instead of leaking secrets, messes with the Jenkins box.
