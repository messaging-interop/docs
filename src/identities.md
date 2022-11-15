# Identities

In this architecture, users will have many contextual identities and, will want to unify or fork those identies and the associated conversations.

Attestations allow people to assert relationships between cryptographic identities for which they hold the private keys. There are two kinds of attestations, with proposed implementations that require security review. Suppose there are identities A and B represented by the public keys \\(A_{pk}, B_{pk}\\), and corresponding private keys \\(A_{sk}, B_{sk}\\). Let \\(Sign_{sk}(M)\\) represent the signature of message \\(m\\) with private key \\(sk\\)
- Sequential - B follows A
	- Proposal: \\(Sign_{B_{sk}}(B_{pk}||Sign_{A_{sk}}(B_{pk}))\\)
- Parallel - A is equivalent to B
	- Proposal: \\(Sign_{A_{sk}}(B_{pk})\ ||\ Sign_{B_{sk}}(A_{pk})\\)

We should consider trust to be transmitted directionally in a Sequential relationship, and bidirectionally in a Parallel relationship
- If B follows A
	- I can trust B with the union of trust in A and B
	- I learn nothing new about trust in B
- If A is equivalent to B
	- I can now trust A and B with the union of trust in A and B

Messages from a key B that follows a key A should be merged chronologically with messages from A in a conversation history. Messages from a key B that is equivalent to key A should not be.

These attestations can be used in the following ways:
- Rolling keys over time
	- the replacement key should follow the key being replaced
- Linking a new identity to an existing one.
	- Alex and Blair met and exchanged keys \\(IPK_A\\) and \\(IPK_B\\), but Alex wants to prove they also have a attested identity \\(IPK_{A_D}\\) from a Directory Server
		- Alex should generate a new \\(IPK_{A-Merged}\\) and present attestations that it follows \\(IPK_A\\) and \\(IPK_{A_D}\\)
- Forking threads
	- If Alex wants to fork a conversation they have with Blair across identities  \\(IPK_A\\) and \\(IPK_B\\), Alex can generate a new identity key \\($$IPK_{A'}\\), and assert an equivalence between \\(IPK_A\\) and \\(IPK_{A'}\\). From Blair’s point of view, \\($$IPK_{A'}\\) should be a new thread, inheriting whatever trust they had in \\(IPK_A\\)

### Conflicts
After issuing an attestation that B follows A, clients should delete the private key for A, and sending chain keys derived from A. (They should keep the receiving keys for some period of time). Future messages should be sent with chain keys derived from B.

Still, conflicts can arise if clients receive conflicting attestations that B follows A, and C follows A. These can result from software bugs, a [device with outdated state](devices.md) , or compromise of private keys. A device with outdated state can heal its state by reaching consensus with a device with newer state about ordering of keys. E.g. by issuing an attestation that C follows B.

Otherwise, these conflicts must be resolved by the user. There are two cryptographic identities claiming to be Blair, which one is correct? Examples of how this might arise:
- Alex introduces Blair to a backup identity \\(A’\\) that follows \\(A\\)_.  The backup service is compromised and an attacker attempts to impersonate Alex using $$A’$$. There are now two claims to Alex’s identity.
	- The presumption could be that a use of a backup identity A’ while A is active is fraudulent and should be assumed to be malicious. But Alex’s device might have been compromised, and is attempting to restart their relationship from a backup.
	- Technology can’t resolve this conflict - Blair has to socially resolve this conflict between the two claims to Alex’s identity, for example, by verifying keys in person
- An external attestation is a way of resolving this conflict.
	- It can also be a source of conflict. Blair has a relationship with Alex, and Alex has asserted and equivalence with the public key for \@Alex on example.com's Directory Server at a point in time.
	- If example.com's Diretory publishes a new public key for \Alex on Twitter, the association is no longer valid, and we have a conflict
		- Alex can heal the conflict by issuing an attestation with the new public key
		- The conflict may also arise from an account takeover on the external service rather than a compromise of Alex’s device. This conflict also needs to be resolved socially.
