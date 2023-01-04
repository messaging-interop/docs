# Recovery

We can use the notion of introduction to provide a recovery mechanism that does not depend on a user secret.

Suppose Alex and Blair have a relationship, based in identity (public keys) \\(A\\) and \\(B\\)

When a user Alex enlists a recovery service \\(R\\) to recover a relationship with Blair, they are effectively asking \\(R\\) to reintroduce them as a new identity \\(A_r\\). If this recovery flow is to be resilient against device compromise, then \\(A_r\\) cannot be known ahead of time (or a device compromise would give the attacker access to \\(A_r\\) as well). 

## Preparation

Alex and Blair can pre-arrange recovery in the following way:
- Alex creates a recovery identity key pair \\(A_r\\), a (long-lived) recovery address(es) \\(Addr_A\\), and a recovery identifier \\(ID_A\\) for a key directory \\(K_A\\), and sends the public key, address, key directory, and recovery identifier to Blair.
	- Blair symmetrically generates \\(B_r\\), \\(Addr_B\\), \\(ID_B\\) for \\(K_B\\), and sends them (excluding the private key) to Alex.
	- (Implicitly the public identity key is accompanied by some pre-keys as needed for a key agreement protocol throughout this description)
- Alex can then store the Recovery Particle \\(RP_B\\) with a trusted party in plaintext:
	- Blair's name
	- A recovery identity key that Blair has today \\(B_r\\)
	- Recovery Address(es) for Blair \\(Addr_B\\)
	- A way to get a future identity for Blair: \\(ID_B\\) at \\(K_B\\)
	- The key directory entry that Blair expects to receive messages from: \\(ID_A\\)

The first exchange limits the exposure of data about Alex and Blair's current communications - only the recipient's name, receiving service agent, and key directory.

## Recovery

1. Alex can then recover from compromise or data loss by authenticating with \\(R\\) to retrieve \\(RP_B\\), generating a new identity \\(A'\\), and registering the public key for \\(A'\\) with \\(K_A\\) for the identity \\(ID_A\\).

2. Alex can then compose an initial key agreement message to \\(B_r\\), from \\(A'\\) and send it to \\(Addr_B\\)

3. Blair, on receipt of a message at \\(B_r\\), can consult \\(K_A\\) for Alex's new identity public key under the entry for \\(ID_A\\)

If the key directory also allows the registration of a new address, then \\(K_A\\) and \\(K_B\\) can jointly provide recovery from data loss by both parties, as \\(ID_A\\) and \\(ID_B\\) effectively form a pre-arranged rendezvous.

