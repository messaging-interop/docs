# Communicator: a protocol for durable, consensual conversations

## Problem

Messaging today has a fundamental architectural problem in that we overload addresses (e.g. phone numbers or email addresses) as identity. This unexpectedly fractures conversations across different transport mediums, and when people change addresses. 
People have to manually unify conversations across addresses by maintaining a contacts database that maps people to their many addresses. Because addresses convey identify, it exposes metadata in transit about who is talking to each other.

Messaging also has a conceptual problem in that we assume, implicitly if not explicitly, that a person has one identity in any system. From those identities, we derive the concept of messages by directing data from one identity to some set of recipient identities, and we can then collect messages into conversations by organizing individual messages in relation to each other, or by the set of participating identities.

This construction leads to an architectural default that anyone can message anyone else, enabing spam and harassment, as it is costlier for a victim to change addresses than for an attacker to evade sender-identity blocking. In turn, this incentivizes systems to be hostile to using multiple identities. There is no architectural distinction between a solicited and unsolicited message, so systems rely on data about who we have messaged, and who we know (contacts databases) to infer the notion of a consensual relationship. 

The Communicator architecture addresses these problems by making a relationship between two people the core concept, and decoupling identity at the application layer, from the addresses used for transport. 

## Protocol

The Communicator architecture is a messaging protocol that enables people to asynchronously create relationships and  exchange messages in those relationships. It extends the core idea of Double Ratchet key management - that two parties protect their communications with shared secrets that are updated with each message exchange - to the addresses that they use to deliver those messages to each other. 


Two parties Alex and Blair consensually create a Communicator relationship by exchanging data that allows them to create an authenticated encryption session, and transport messages in that session with each other. This dynamic state that allows Alex and Blair to securely exchange messages is stored in a data structure we call a Particle, to reflect the intuition that Alex and Blair each have a corresponding data structure, entangled through message exchange.

### Continue existing conversations
Once Alex and Blair have established a Communicator relationship, they can use it to send messages in the following way:

![sending a message in a relationship](img/message-exchange.png)

Alex and Blair each enlist the help of multiple, persistently online services to perform the asymmetric roles of sending and receiving messages, each acting as one party's agent.  Alex's state (Particle) for their relationship with Blair contains information about Blair's [receiving service agents](reference/receiving-service.md), and addresses for Blair at each of those receiving services.

When Alex has a message to send to Blair, Alex attaches to the plaintext any updates to their Particle state (e.g. updates to Alex's receiving services or addresses), and encrypts the message to Blair using the agreed-upon key schedule.

To transmit this ciphertext, Alex chooses one of their sending services and one of Blair's receiving services. Alex can then route the message by presenting Alex's chosen sending service:
* The message ciphertext
* A URI for the chosen receiving service
* an address for Blair at that receiving service

The sending service does not need to know Blair's address, so Alex encrypts it with a public key published by the Receiving Service.

### Start new relationships
Initializing a Communicator relationship requires a bidirectional exchange of keys and addresses. In the simplest manner, Alex and Blair may simply exchange these directly between their devices through a local, peer to peer transport such as Bluetooth.

In many cases, Alex and Blair will rely on a persistently online Directory Service to broker an asynchronous, remote exchange. 

#### Brokered Directory Service
The most full-featured directory service can vend keys (long-term identity key and prekeys - call this a KeyPackage) and addresses for names within a namespace they assert authority over.

![brokered directory service](img/brokered-directory.png)

This architecture resembles the deployment of most end to end encrypted messaging services today (Signal, iMessage, WhatsApp).

The directory service in this role grants permission to contact, and may subject this permission to rules within its own namespace.
#### Attesting Directory Service
Directory services can also attest to bindings of identity (keys) with names in their namespace without granting permission to contact by issuing addresses. This is useful for establishing trust for relationships established through an intermediary - either an insecure medium, or via a person to person introduction.

![attesting directory service](img/attested-directory.png)


## Considerations

This architecture is alike SMTP in that senders and recipients make independent choices of agents for sending and receiving messages, analogously to mail submission agents and mail delivery agents. Unlike SMTP, these agents only handle ciphertexts, and clients are responsible for storage of plaintext messages. They may choose to enlist the aid of additional services to help synchronize this storage across devices, or provide recovery in case of device loss.

