# Sender's Service Agent

The function of the Sender's Service Agent (SSA) is to act as the sender's online agent for storing data - either transiently (queueing messages, storing message attachments) or persistently (group state). SSA's should provide technical assurances against reidentifying metadata by not handling identifiable information about the sender, and using unlinkable tokens to authorize use of the service and rate-limit resource usage.

## Message Queueing
In the message transmission path, the SSA is an intermediary between a person's computing devices, and the recipient's service agent, providing reliability and anonymity. 

## Attachment Storage

The QSS also serves as the sender's agent for storing encrypted ciphertexts that are too large to fit in the message protocol, for later retrieval by the recipient or their agent.

## Group Coordination

As discussed in [Groups](/groups.md), it can be helpful for for stability and coherence for groups to have consistent state and ordering of messages. MLS requires, for example, a Delivery Service provide consistent ordering of messages. This function can be provided by a sender's service.

Typically, on group creation, the group creator will nominate their sender's service agent as the service agent for the group they create. 



