# Interoperability

People use diverse software and services, and it is important to allow people make independent choices about the software agents they use to send and receive messages - just as they do with email and telephony. The key points of interoperability in this architecture are:
* Between senders' and recipients' client software that encrypts and decrypts messages under this architecture and maintain syncronized proton state.
* Between Sending and Receiving agents to transmit ciphertexts for a destination address
* Between client software and Sending and Receiving Agents
	* This allows one client to use multiple sending and receiving agents to provide reliability and privacy
