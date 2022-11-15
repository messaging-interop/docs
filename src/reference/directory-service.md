# Directory Service

This Directory Service asserts authority over names \\(n\\) within their namespace \\(N\\). This namespace could represent members of an organization with a defined membership (e.g. a school or business), or could be a social media context where people create avatars and connect to other members to have conversations (e.g. a matchmaking service - i.e. a dating app)

After authentiating as a user \\(n\\), a person can perform the following operations with the Directory Service:
* register an identity public key for \\(n\\)
* replace the existing identity public key for \\(n\\)
	* optionally, provide proof that the user still has posession of the previous identity private key by signing the new identity public key.
* query for the quantity of pre-key and address packages available on the server
* upload additional pre-key and address material

The Directory Service also provides the following services:
- Query for the identity public key for \\(n\\)
- Request permission to contact \\(n\\)
	- The Directory service can return an address
	- The directory service may choose to authenticate such requests. E.g. the social graph of dating services are not completely open, but are deliberately restricted by the operator to restrict choice.

The directory service should provide proof of its honesty by implementing key transparency. 