# Recovery

There are two different recovery challenges - recovery of message history, and recovery of relationships, since they are now dependent on private data stored on users' devices.

There is a spectrum of techniques for backing up data, with tradeoffs between privacy and recoverability. One may store data with a recovery service, protected with a user secret to prevent the service from accessing that data. But that data is irretrievable if the user forgets their secret. There are enhancements to this approach where I can enlist trusted parties to hold parts of the secret, so that no one party can access my data[^1].

People may choose different points on this spectrum for themselves, for their message history, and for their relationships.

We can introduce an additional recovery mechanism for relationships that is robust against losing all my data and forgetting my recovery secret(s), but has safeguards against the escrow party impersonating me - using the notion of introductions:

### Escrowed Relationship Backup
Alex has a relationship with Blair that is based on cryptographic secrets stored in their respective protons. To protect against the loss of that data, Alex can introduce Blair to a future Alex \\(A'\\), and escrow \\(A'\\)'s keys with a third party.

The data Alex needs to escrow are:
- A long-term address for Blair.
- Blair’s IPK for this relationship
- A signing key for \\(A'\\)
- A preferred name for Blair

If Alex loses all of their proton data, they can obtain this backup, notify all their relationships of the loss, and use each \\(A'\\) to introduce the relationship to a new device resident identity \\(A_r\\).

As with introductions, remote parties should only accept the use of this placeholder key \\(A'\\) for introduction to a new identity. The escrow party cannot impersonate the Alex, but can falsely claim to be a future, post-recovery Alex \\(A_r\\). This should be easy to repudiate if Alex is still online. Recipients of such a recovery message should immediately consult \\(A\\), and until they can verify \\(A_r\\), consider the possibility that the escrow party had inserted themselves in the conversation.

If Alex’s relationship with Blair is based on a publicly attested identity (i.e. a Twitter handle), Alex can instead rely on Twitter for identity recovery, and to publish a new IPK and address(es) for Alex.

Escrow parties, then, learn about the remote parties that Alex communicates with. They may be able to associate those backed up contacts with outgoing messages to the long-term address, but by design there is nothing to associate those contacts with user’s incoming messages. Receiving services are natural candidates to escrow these contacts. Using two separate receiving services as the system intends, prevents the escrow party from also denying receiving service to prevent repudiation of a misused backup.

[^1]: Apple's account recovery contact: https://support.apple.com/guide/security/account-recovery-contact-security-secafa525057/1/web/1
