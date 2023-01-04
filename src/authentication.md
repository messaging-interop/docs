# Authentication

Who is at the other end of a relationship?

In the Communicator Architecture, authentication of the other party is rooted in the formation of the relationship, and may be supplemented by later [attestations](identities.md) with other identities or sources of identity, such as a key directory

## How did this relationship start?

A relationship is formed by an initiating party Ira, for a recipient party Reed: 
1. Ira generates an identity key (along with other key material), addresses, and sends them as a relationship invitation to Reed. 
2. Reed uses this data to send an initial message to Ira. [^1]

Ira and Reed's authentication of the other party is rooted in how these first messages are transmitted and what parties are involved.

- Direct Exchange 
	- Suppose Ira and Reed have an existing means of secure bidirectional data exchange. Ira sends a relationship invitation to Reed; Reed sends their identity public key in response over this channel, and the initial message over the Communicator architecture. In doing so, the trust in each other's identity is initially equivalent to their trust in this bidirectional channel.[^2]
- Introduction by an intermediary
	- Ira may allow an intermediary server \\(S\\) to make introductions for them at the server's discretion. Ira can upload an identity public key, prekey packages, and address packages (or give the server a way to diversify addresses). The server can then vend the relationship invitation message at its discretion, asserting some identity for Ira. A user Reed who obtains a relationship invitation in this way knows they are communicating with an entity that \\(S\\) asserts is Ira. Ira, though, only knows that it Reed is someone who found them through \\(S\\). Reed may send some additional information about their identity - e.g. that their identity public key is registered on \\(S\\) for some identity \\(R\\).
		- If both Ira and Reed use identities registered with \\(S\\), this resembles the authentication for deployed telephony-indexed E2EE messaging systems
		- This also allows for relationships with asymmetrical trust. \\(S\\) may be operated by a newspaper, through which Ira may solicit tips. Reed may ask \\(S\\) for an introduction to Ira, without initially revealing identity information about themselves, but is able to place some trust in \\(S\\) as the entity vouching for Ira's identity.
	- If Sam has a relationship with Ira and Reed, Sam can [introduce them](introductions.md). Ira and Reed initially only know each other through Sam (and must assume that Sam may have MITM'd this relationship), until they have otherwise verified each other's identity

## Additional sources of Authentication

- Verification
	- Ira and Reed may verify their identity public keys through a trusted out of band channel
- Association with other identities
	- Participants may use the [attestation](identities.md) operations to prove associations with identities. For example, Ira and Reed, who started a relationship with an in-person exchange, may also attest equivalence to identities in a public key directory. 


[^1]: To use X3DH as an example, the relationship invitation contains Ira's identity key, prekeys and addresses. Reed's initial message corresponds to the X3DH initial message, transmitted through the [Communicator architecture](architecture-overview.md) of sending and receiving agents

[^2]: Operating systems are increasingly restrictive (with good reason) of mobile apps' access to low-level local networking protocols that would enable bidirectional exchange of data. A sketch of how one might implement this exchange, akin to the [CTAP2](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html) secure channel between an authenticator and client:
- Ira uploads the relationship message to a server, and generates a QR code with a url to fetch the message, and a hash of the message
- Reed scans the QR code, fetches the message, and verifies the hash over the message.
- Concurrently, Reed broadcasts a BLE advertisement of a new Identity public key generated for this relationship
- Reed sends an initial message in reply over the Communicator architecture