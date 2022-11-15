# Permission to Contact

## Problem

Existing approaches to mitigating unwanted communications in an architecture that defaults to accepts messages from any sender, rely on collecting and processing message contents or metadata, or push work on individuals to filter incoming messages, sender identities, and message requests.


## Design

The proton architecture separates communications in an existing relationship, from mechanisms of starting new relationships. This allows the former to minimize transport metadata, while allowing people to control when they start new relationships.

People already start relationships (or invite them) when they give out an email address, or post it publicly and solicit messages. The proton architecture fulfills these same transactions, in the following ways:

* Direct exchange
	* In the way that people today give their email address to a specific person or business, they can instead send an identity public key, and an initial set of addresses as a way of starting a new relationship. 
	* This can be done over a local transport (Bluetooth or NFC), through a secure channel (signing up for an account on a website), or over an insecure legacy transport.
* Introduction
	* Someone I already have a relationship with can introduce me to a third person. Discussed further in [Introduction](/introductions.md)
	* I can entrust a directory service to broker introductions according to some rules (members of the same organization, degrees of separation in a social graph, etc)
		* This may be an open solicitation to new conversations. A newspaper might vouch for the identity of its journalists by operating a directory service, and provide a way to start new conversations with them. 

## Bridging

People already have many existing conversations, in many mediums, and it will be important to bridge conversations into this new architecture. This is typically why messaging architectures default to accepting any incoming messages. Bridging techniques are outside the scope of this document. 
