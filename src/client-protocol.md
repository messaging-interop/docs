# Client API

## Problem
For Alex and Blair to communicate using different sofware across an interoperable standard, the standard must be stable. This standard should itself support the kinds of payloads that already exist or have been standardized - short texts, longform prose (email), and call invitations.

This is in tension with people's desire to communicate in new and novel ways - for recent examples, with short-form video or ephemeral messages. There are many circumstances where Alex and Blair need to agree on an app to do specific things within the relationship. Innovation in this space has long relied on new apps' access to people's social graphs by granting access to a user's contacts, with corresponding privacy harms and abuses.

## Design
We can fulfill this desire in a more privacy-preserving way by exposing on-device APIs that allows people to use new apps with their existing social graph, while abstracting away the implementation of message transmission. Examples of use cases and privacy-preserving API's:

- Custom messaging clients
	- People may want to use different user interfaces (possibly at the same time) to send and receive messages, just as people want to make an independent choice of mail app from their mail service. A new messaging interface app can request permission to access a person's messaging database, and to an API to send messages. This doesn't expose keys or addresses, and operating systems can help prevent exfiltration of message plaintext with sandboxing, just as they do for app extensions that handle sensitive data, such as keyboard extensions.
- Novel Payloads
	- Lots of media types that people want to exchange are split across multiple services - e.g. payments, ephemeral messages, or call invitations. These use cases can be accommodated with API's that allow these apps to 
		- register for a payload type
		- supply a payload within the hosting app's compose window
			- This payload can include a fallback message indicating what app can be used to display the payload
		- invoke a compose window from their app
		- send messages programatically in relationships that have already been used with the app.


	- In the B2C case, this could include 2FA notifications. For example, I sign up for an account on a bank's website, using a Communicator identity. This Communicator identity lets the bank send me short (alerts) and long (promotional mail) text messages. I can also allow the bank's app to process app-specific payloads, for example 2FA requests that I must authorize in their app after authenticating.