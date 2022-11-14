# Groups

A group is a set of people who want to talk amongst themselves. Whereas the Proton data structure for a 1:1 relationship containes keys and addresses for person at the other end of the relationship, the Group Proton data structure contains the same data (keys and addresses) for all other participants. For a Group \\(G\\), the member \\(G_c\\) would have the following data structure

- For each other group member \\(GP_i\\):
	 - A human-readable identity:
		- A string representing a preferred name
		* (Optional) pronouns
		* (Optional) A profile picture - could be emoji, monogram, or photo
	- An Identity Public Key (\\(IPK_i\\))
		* Optionally, an external resource that attests to a link between $$IPK_i$$ and some external identity (e.g. a scoped username - B on Twitter)
	- A list of receiver's service agents (minimum 2), that each contains:
		- A well-known url representing the service identity and configuration:
			-  E.g. signal.org/.well-known/Proton.json
		- A long-term delivery address (this facilitates recovery)
			- With a guaranteed valid until date
  		- Ephemeral delivery address(es) for \\(P_b\\)
		    - With a short expiry date
- An Identity private key for \\(IPK_c\\) corresponding to the \\(IPK_i\\) held by other group members
  - A set of associations of \\(IPK_c\\) with other related protons (relationships)defined by cryptographic attestation - see [Identity].
- An Identity private key for \\(GP_c\\) corresponding to the \\(IPK_c\\) held by other members of the group
- Message Security state variables
	- discussed later in this document

This is sufficient for a group to talk amongst themselves - agreement with other participants on who (defined by Identity Public Keys) is a member of this group, and how to deliver messages to them. 


## Group Management
There is a spectrum of approaches to handle consistenty of group membership and message ordering, here are a few examples:

### Fully Decentralized
Each message has a set of recipients. Membership is implicitly changed by changing the recipient list for new messsages. Messages are not fully ordered - messages can be relatively ordered to indicate a reply, 

This is how email and iMessage function

### Centralized Membership, Decentralized message delivery
Members of a group agree on an authoritative source of group membership, but do not rely on a single server for 

Signal's private groups function in this way. 

### Centralized Membership and Message
Chat rooms, mailing list, or Slack, are examples of this. There is a single source of truth for message ordering - participants compete to edit a collaborative, append-only document that is the room's message transcript. The room (and implicitly, the room operator) is the fundamental entity, not the set of participants that make up the group.

These different approaches are suitable, roughly, for groups of increasing size. Groups can be handled in the following 2 ways with the Proton architecture:
## Lightweight Groups
These groups operate in the fully decentralized way. The Message security state is a pairwise double ratchet state for each member of the group.

## Coordinated Groups
These groups leverage a server agent to store the group's state (e.g. membership), and as the single Sender's Service Agent for the entire group. This allows it to perform the message ordering required by Message Layer Security. Here, the message security will be provided by MLS, not pairwise Double Ratchet.

Coordinated groups are robust against failure or denial of service by the server agent by falling back to a lightweight group, and can nominate another server agent to take over. 


The Application layer can choose between these architectures when forming a group, and can add coordination to a lightweight group at anytime. 