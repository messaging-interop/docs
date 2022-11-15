# Receiver's Service Agent

The Receiver's Service Agent (RSA) helps recipients protect the privacy of their relationships by providing a supply of addresses at which they can receive messages.

Receiving services must also help senders hide addresses from their sending service, by publishing a public encryption key so that senders can encrypt addresses to the receiving service.

Our reference RSA provides the following functionality to clients who present unlinkable authorization tokens:
* Provide a supply of seed addresses 
* Allow clients to derive new addresses from those seed addresses without interacting with the RSA 
* Allow clients to query for messages delvered to those seed addresses
* Allow clients to register for immediate notifications for newly received messsages (e.g. with a push token or by opening a WebSocket)

The reference RSA provides address derivation by use of randomized public key encryption.

The RSA also, as specified in [Interoperability], allows senders to encrypt addresses to the RSA so that senders can produce derived addresses indistinguishible to the Sending service without receiving new addresses from the recipient.


## Analysis and alternative approaches

The RSA will receive messages with addresses that have two layers of randomized encryption applied, to prevent linking messages to a particular sender or recipient. The outer layer renders addresses indistinguishible to sending services. The inner layer renders addresses indistinguishible to senders so that e.g., senders cannot compare addresses to determine if they are talking to the same party.

This double encryption produces somewhat large addresses, which is an opportunity for optimization. But fundamentally, we want to have a large, sparse address space so that users can use arbitrarily many addresses, while making it difficult to enumerate valid addresses. 

The RSA holds the private keys to decrypt these addresses and reveal the seed address, as it must to resolve indistinguishible addresses to a particular recipient. The primary privacy protection is that unlinkable tokens separate the recipient from any identifiers the client presented to the Authorization Service. 

WebSocket connections may be disassociated from the user's IP address by the use of a Multi-Party Relay (MPR) such as iCloud Private Relay or INVISV Relay[^invisv]. If users wish to receive real-time notifications for messages by registering a platform push token, these are pseudonymous identifiers that may be linked back to a user with the aid of the platform.

RSA's may take additional steps to reduce their knowledge of message metadata, possibly with some efficiency tradeoff. For example, by loosening the constraint that they know the specific user each address maps to, to associating each adddress with a paging set. It may notify a set of clients, who can decide if they wish to fetch the message contents, e.g. through an MPR.

Importantly, these additional steps do not require any manual intervention from senders. Because addresses in this architecture are defined by recipients and are automatically updated when senders receive new addresss from recipients, people can change RSA's and seamlessly transition between multiple RSA's.


 [^invisv]: [INVISV Relay: Providing Multi-Hop Privacy for All](https://invisv.com/articles/relay_for_all.html)