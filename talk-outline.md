All the movement towards releasing software continuously puts us in a tricky position. Developers want to move fast, ship features quickly. They cut a few corners doing this, including:

* Sticking credentials for services/infrastructure directly in their repositories (BAD, sometimes accidental)

For development sanity, they perform integration tests using continuous integration services like Travis CI.

* Performing integration tests

Need a few cheap servers? People love to make development easier by leaving credentials in public repositories.




Cover a typical build pipeline

* List steps



We'll cover a typical build pipeline for an open source project.


Three tiers of hijacking infrastructure:


Jenkins better be gated.


Lies, Damned Lies, and API Calls.


AWS does searching for credentials?
