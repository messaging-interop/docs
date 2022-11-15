# Multiple Devices

People often have multiple computing devices, and want to use them to send and receive messages as the same identity. Messaging architectures today, generally model different devices as distinct members of messaging groups, and rely on the application layer to hide each participant's devices.

In a federated architecture, people cannot rely on other parties' software to hide the devices they use, and how they use them. Our messaging architecture should reflect the way that people expect to converse, which is that they present a uniform identity to other parties, regardless of which device they're using.

This is an area for future work, a sketch follows of how this might be implemented:

## Constellations of devices emulating a single, ratcheting identity
This assumes users have a set of devices which are running the same client software managing key state, and which can exchange end to end encrypted messages amongst each other. In addition, these devices may also enlist a server to store and synchronize data, protected with keys that are only held by these devices.

Using double ratchet as an example, all of Alex's devices emulate a single ratchet identity by sharing private keys amongst themselves, copying peer devices on messages sent to the remote party, and fanning out incoming messages to all of Alex's devices. This works under ideal conditions, but the concept needs adaptation to be resilient against dropped and delayed messages, and race conditions. Key challenges will be:

- Resolving ratchet conflicts
	- If two devices perform a ratchet at the same time, they may fork the ratchet chain. 
- Robustness with offline devices
	- One edge scenario is that a device sends a message with a ratchet public key, and is immediately destroyed before it can share the ratchet private key with its peers, locking peers out of the conversation.

From the standpoint of a theoretical recipient that receives and processes all messages from each of Alex's devices, these messages contribute to a virtual ratchet state - that may fork into a tree instead of an ordered chain. Each of Alex's devices is aware of a subset of this virtual state, filling in the details as it receives messages from peers and the remote party, or consults their sync service. Each device, acting on the possibly incomplete information, has to decide on actions as it sends and receives messages - e.g. which branch of a forked chain to use.

The failure modes raised above hint at an explicit window of valid ratchet states, representing some depth of leaves of the tree, to ensure devices can send messages without depending on data from their peers. To keep this window small, devices must aggressively prune the tree, and narrow the depth of valid states. The former can be done with agreement on an ordering of states. The latter could be done with an explicit signal to the remote party that a particular DH key is the oldest one that should be honored - e.g. if the corresponding private key has been committed to (end to end encrypted) cloud storage, or been received by N other peer devices.