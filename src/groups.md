# Groups

A group is a set of people who want to talk amongst themselves. Whereas the Particle data structure for a 1:1 relationship containes keys and addresses for person at the other end of the relationship, the Group Particle data structure contains the same data (keys and addresses) for all other participants. For a Group \\(G\\), the member \\(G_c\\) would have the following data structure

- (optional) A name for the group
- (optional) A profile picture for the group
- For each other group member \\(GP_i\\):
	- The group member's preferred presentation:
	  - A string representing a preferred name
	  - Optionally, pronouns
	  - Optionally, a profile picture
	- An Identity Public Key (\\(IPK_i\\))
		- Optionally, an external resource that attests to a link between \\(IPK_i\\) and some external identity (e.g. a scoped username like an email address)
	- A list of [receiver's service agents](reference/receiving-service.md) (minimum 2), that each contains
	  - A URL representing the service identity and a means to obtain the service's configuration:
	    -  E.g. `example.com/.well-known/Communicator.json`
	    (analogous to an MX record)
	  - A long-term delivery address for \\(P_b\\)(this facilitates recovery)
	    - With a guaranteed valid until date
	  - An ephemeral delivery address for \\(P_b\\)
	    - With a short expiry date
- An Identity private key for \\(IPK_c\\) corresponding to the \\(IPK_i\\) held by other group members
  - A set of associations of \\(IPK_c\\) with other related Particles (relationships)defined by cryptographic attestation - see [Identity].
- An Identity private key for \\(GP_c\\) corresponding to the \\(IPK_c\\) held by other members of the group
- Message Security state variables
	- discussed later in this document

This is sufficient for a group to talk amongst themselves - agreement with other participants on who (defined by Identity Public Keys) is a member of this group, and how to deliver messages to them. 


## Group Management
There is a range of approaches to handle consistenty of group membership and message ordering; here are a few examples:

### Fully Decentralized
Each message has a set of recipients. Membership is implicitly changed by changing the recipient list for new messsages. Messages are not fully ordered - messages can be relatively ordered to indicate a reply. Email and iMessage function in this way (as do legacy Signal groups). Race conditions may cause groups (and conversations) to fork.

### Centralized Membership, Decentralized message delivery
Members of a group agree on an authoritative source of group membership (and other group metadata like a name), but do not rely on an external resource (like a server) to adjudicate consistent message ordering. Signal's private groups function in this way. 

### Centralized Membership and Message
Chat rooms, mailing lists, and Slack, are examples of this. There is a single source of truth for message ordering - participants compete to edit a collaborative, (mostly) append-only document that is the room's message transcript. The room (and implicitly, the room operator) is the fundamental entity, not the set of participants that make up the group.

These different approaches are suitable, roughly speaking, for groups of increasing size. The Communicator architecture has the following two approaches to group management:
## Lightweight Groups
These groups operate in the fully decentralized way. The Message security state is a pairwise double ratchet state for each member of the group.

## Coordinated Groups
These groups leverage a server agent to store the group's state (e.g. membership), and as the single Sender's Service Agent for the entire group. This allows it to perform the message ordering required by Message Layer Security. Here, the message security will be provided by MLS, not pairwise Double Ratchet.

Coordinated groups are robust against failure or denial of service by the server agent by falling back to a lightweight group, and can nominate another server agent to take over. 

The Application layer can choose between these architectures when forming a group, and can add coordination to a lightweight group at anytime. 