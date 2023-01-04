# Relationships

Relationships, not identities, are the fundamental unit of this architecture. Identities are contextual to each relationship, and users can choose how each of those contextual relationships relate to fixed or public identies, such as those published in directories.

A **relationship** \\(R\\) between Alex and Blair is a pair of data structures \\(P_a\\) and \\(P_b\\), stored by Alex and Blair respectively, that represent each end (role) of the relationship, and contain the data needed to communicate with the other party. Let’s call  \\(P_a\\) and \\(P_b\\) **Particles**, to reflect the intuition that they are entangled (through message exchange) and reflect shared state.

\\(P_a\\) consists of:
- The remote party's preferred presentation:
  - A string representing a preferred name
  - Optionally, pronouns
  - Optionally, a profile picture
- An Identity Public Key (\\(IPK_B\\)) for \\(P_b\\)
  - Optionally, an external resource that attests to a binding between \\(IPK_B\\) and some external identity (e.g. a scoped username like an email address)
- An Identity private key for \\(Pa\\) corresponding to the \\(IPK_A\\)held by \\(P_b\\)
- Message security state variables
  - These would be Double Ratchet chain keys for 1:1 conversations
- A list of [receiver's service agents](reference/receiving-service.md) (minimum 2), that each contains
  - A URL representing the service identity and a means to obtain the service's configuration:
    -  E.g. `example.com/.well-known/Communicator.json`
    (analogous to an MX record)
  - A long-term delivery address for \\(P_b\\)(this facilitates recovery)
    - With a guaranteed valid until date
  - An ephemeral delivery address for \\(P_b\\)
    - With a short expiry date