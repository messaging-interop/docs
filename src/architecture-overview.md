# Architecture & Protocol Model

## Problem

A fundamental problem in messaging today is that we overload addresses (e.g. phone number, email address) as identity. This makes conversations brittle across changes of address, cumbersome to bind multiple addresses to a single identity, and exposes metadata in transit about which identities are communicating to each other.

Moreover, the use of static addresses makes it architecturally easy to spam or hararass users, since it is costlier for a victim to change addresses than for an attacker to evade sender-identity blocking.

The purpose of the Proton architecture is to decouple identity from the addresses used for message transmission. The Proton architecture allows two parties to exchange encrypted messages, using shared secrets to protect message contents and metadata in transit.

## Protocol

The Proton Architecture extends the core idea of Double Ratchet - that two parties use shared secrets to derive a schedule of message encryption keys, and update their shared secrets as they exchange messages - to the addresses that they can use to deliver those messages to each other. Two parties Alex and Blair that have shared keys and addresses, are said to have a **Proton Relationship**, and we call the data structure that each has to represent the set of keys and addresses a **Proton**, to reflect the intuition that it mirrors the remote party's data structure and are maintained in synchronous state through message exchange.

### Continue existing conversations
Once Alex and Blair have established a proton relationship, they can use it to send messages in the following way:

![sending a message in a relationship](img/message-exchange.png)

Alex has a set of interchangeable sending services that act as Alex's agent to accept messages from a device, store, and foward messages to a recipient's receiving service. Alex's Proton for their relationship with Blair contains a set of addresses at receiving services where Blair is expecting messages from Alex. 

Alex chooses one of their sending services and one of Blair's receiving services, and routes a message to Blair through this path. This message contains in it new addresses that Blair should use for sending messages to Alex. 

### Start new relationships
Initializing a proton relationship requires a bidirectional exchange of keys and addresses. In the simplest manner, Alex and Blair may simply exchange these directly between their devices through a local, peer to peer transport such as NFC or Bluetooth.

In many cases, Alex and Blair will rely on a persistently online directory service (DS) to broker an asynchronous, remote exchange. 

#### Brokered Directory Service
The most full-featured directory service will vend keys (long-term identity key and prekeys - call this a KeyPackage) and addresses for names within a namespace they assert authority over.

![brokered directory service](img/brokered-directory.png)

The directory service in this role grants permission to contact, and may subject this permission to rules within its own namespace.
#### Attesting Directory Service
Directory services can also attest to bindings of identity (keys) with names in their namespace without granting permission to contact by issuing addresses. This is useful for establishing trust for relationships established through an intermediary - either an insecure medium, or via a person to person introduction.

![attesting directory service](img/attested-directory.png)


## Considerations
\[Potential section on data recovery\]
