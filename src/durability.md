# Durability

People expect their conversations to be resilient against the loss of a device.

There are two aspects of recovery:
- Identities and addresses of people I was conversing with
	- This data is typically backed up on its own as a contacts database, or implicitly with message history
- The ability to communicate with identities I previously used


Today, the identities I use are wholly under the control of the services I use. This is very helpful if I lose my device or forget my passwords, and convince these services to give me control of the same identity on a new device. It's somewhat less helpful if an attacker poses as me to convince the service to "recover" access to those identities.

It is long-standing wisdom that people cannot reliably protect secrets; the diversity of crypto wallets, their failure modes, and people's choices of which models they are comfortable with, are illustrative examples of the frontiers of people's ability to protect secret data that they use.

But to surrender control of our identities wholly to services overlooks the broad availability of data recovery options now deployed that place [limited](https://signal.org/blog/secure-value-recovery/) or [shared](https://support.apple.com/guide/security/account-recovery-contact-security-secafa525057/1/web/1) trust in other parties to recover their data. There are increasingly many ways for people to back up their data without completely trusting the backup entity with access to their data.

## Communicator Relationship Recovery

Because identity in the communicator architecture is a subordinate concept to a relationship, we are primarily concerned with the recovery of a relationship between two parties Alex and Blair, from data loss, compromise of private data, or network denial of service. 

The Communicator Architecture is organized on the principle that two communicants Alex and Blair are the primary agents for recovering their relationship. They may (and likely should) enlist the help of a third party - this is especially helpful against compromise of device secrets - but the third party provides only an alternative, not authoritative claim of anyone's identity.

What does that look like in practice?
- Alex's sole device is destroyed. They had backed up their Particle data to a secure store such as iCloud Keychain. They use a secret only they know (their previous device passcode) to restore their Particle data. Alex is able to recover the relationship with only the help of their secure data recovery service. This recovery does not need Blair's cooperation, nor does this change need to be visible to Blair.
- Alex used the mechanism described in [recovery](recovery.md) to coordinate backup identities with Blair, stored with a recovery service \\(R\\) in plaintext
	- Alex's device is destroyed and they forget their recovery secret. They authenticate with \\(R\\), make use of the backed-up identity, and recover their relationship with Blair. The introduction of a third party that was able to impersonate Alex has degraded Blair's authentication of Alex; this degradation must be transparent to Blair, and is accomplished though the use of the backup identity.
	- From Blair's point of view, there is the Alex identity \\(A\\)they were talking to, and the recovery identity \\(A'\\). Both may be in use at the same time - e.g. if either Alex's device, or Alex's backup are compromised.
	- Today, \\(A'\\) replaces \\(A\\). Blair has no way to resume an established messaging session with \\(A\\), and services provide safeguards or transparency against a malicious replacement of identity, advising users to reestablish trust after such a transition.
	- The Communicator architecture does not presume that either of the competing claims to Alex's identity, \\(A\\) or \\(A'\\) should be trusted by default. Both channels are available to Blair to resolve.