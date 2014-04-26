Talk Outline: Is This Your Build Pipe?
--------------------------------------

Your credentials are secure, right? If compromised, you can just revoke them and
everything will be fine right?

People

* Leak API keys/credentials
* Expose secure credentials inadvertently
* Allow build tools to deploy after tests pass (yes, a good thing too)

## Lies, Damned Lies, and APIs

### What can I do with your cloud credentials?

#### Append my SSH Key to one of yours

```
$ nova keypair-list
+------------+-------------------------------------------------+
| Name       | Fingerprint                                     |
+------------+-------------------------------------------------+
| rgbkrk     | 20:2d:22:d3:2a:33:8b:27:40:3c:7d:56:ef:eb:cc:ce |
+------------+-------------------------------------------------+
$ # Concatenate their public key with yours with a newline between
...
$ nova keypair-add --pub-key ~/sneak-key.pub rgbkrk
```

Free root access on new boxes! If you revoke the API key after learning of a
compromise, I still have access later. If your API key is exposed on a fresh
server, I'll make sure to get back on.

#### Spin up new boxes

This is where I run my dogenet.

![](http://i.imgur.com/yyK46nU.jpg)

#### Get access to your data

* Object stores (S3/Blobs/Swift)
* Queues (SQS/CloudQueues)
* Change root passwords on servers
* "Redistribute" your load balancer and DNS entries

Each one of these avenues gives me insight and access into your build network
too. Can I get patches merged in when you deploy?

#### The list goes on

What would you do with these credentials? Is every provider the same? What
insight do you have into what your provider is actually doing?

At DEF CON 19, we saw Ben Feinstein and Jeff Jarmoc show leaked credentials,
potential backdoors, and provide a BackTrack AMI that reached out on first run.

Do you have images you need to protect for your own users? How trusted is your
brand?

### What can I do with the application side of OAuth keys?

Depends on the service, but for quite a few I can

* Masquerade as your application, get people to authenticate with me using your `$TRUSTED_BRAND`
* Generate *your* access tokens, masquerade as you.
* Get a bearer token, use it to perform any application level access
* Keep you above the rate limit, continuously

## How do I get access?

### SecOops: Credentials in public repositories

In the rush to deploy code and get services moving quickly in collaboration, coders will put credentials in public facing repositories.

With a quick search, you too no longer need your own money to mine DogeCoin.
GitHub makes it really easy through their code search API to find the most
recently indexed results.

[Recent AWS keys uploaded](https://github.com/search?o=desc&q=AKIAJ&ref=searchresults&s=indexed&type=Code) (`AKIAJ`)

> I did not completely scrub my code before posting to GitHub. I did not
have billing alerts enabled ... This was a real mistake ... I paid the
price for complacency

Source: https://securosis.com/blog/my-500-cloud-security-screwup

#### Sound the Alarm! Alert the World!

To combat this for the public at large, we're releasing gitsecnanny.

GitSecNanny searches repositories for security oops. The first we're going after
is credentials left out in the open in public repositories.

After finding credentials, we email the original committer and the owner of the
project to let them know how to revoke keys.

{{ Anonymized examples and responses will be shown, as well as statistics about
how often keys are leaked (that we know of) and how many we've helped correct so
far. }}

### Hijacking encrypted secrets

Submit a pull request against a repository that uses encrypted secrets on Travis CI. As simple as writing them out to std(out|err) or even as complex as posting to a webhook.

Easy right?

Travis mitigates this issue by not making the secure env variables available on pull requests from forks.

> Please note that secure env variables are not available for pull requests from forks. This is done due to the security risk of exposing such information in submitted code. Everyone can submit a pull request and if an unencrypted variable is available there, it could be easily displayed.

Props to them for making sure to think about this.

What about forking the repository and adding it to Travis itself? The encryption keys are 1-1 with the repository.

> Please also note that keys used for encryption and decryption are tied to the repository. If you fork a project and add it to travis, it will have different pair of keys than the original.

Is there anything to be worried about then?

Yes. *Code review still stands*. Make sure no one does anything stupid with your "secure" environment variables before merging code in.

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

Those same secrets you could leak while in Travis can be leaked from Jenkins.  Same flow:

1. Find repository that requires credentials in tests
2. Do legitimate work on a feature or bug
3. Include your security oops
...
4. Profit

The great thing about the Jenkins box is that you can poke around as part of the build to see what else is available on the box/give a shell and escalate privileges thereafter.

If you're able to discern if this box is also used to deploy to production, then by all means deploy something to production. ;)

#### Employ more butlers

To protect against this, dev/qa should be separate from staging and production. If possible, builds should run on their own slave boxes. Assume anything on that box can and will be compromised (reduce permissions on API keys).

What happens if that build passes? Can someone just change the test script to pass?
