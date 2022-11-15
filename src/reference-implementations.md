# Reference Implementations

The following sections offer basic implementations of the required components. They assume an authentication service that handles identifiable data (e.g. payment) and allows people to reclaim their account by presenting external credentals (e.g. proof of identification or of payment method).

This reference implementation of the Directory Service consumes identifiable authentication tokens, so that people can reset their directory entries by appeal to toe the authentication service.

The receiving and sending services, however, separate the identity used for authentication, from usage metadata by the use of unlinkable tokens (e.g. Privacy Pass). The authentication service vends tokens, rate-limiting their issuance to prevent abuse. Those tokens can be redeemed with the operational servers. 