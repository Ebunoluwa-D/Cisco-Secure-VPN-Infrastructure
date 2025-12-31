Project: Enterprise Site-to-Site IPsec VPN
Overview
A secure networking project connecting two remote sites (Branch A and HQ B) across a simulated ISP using an encrypted IPsec tunnel.

Tech Stack
Hardware: Cisco ISR 4331 Routers

Security: IKEv1 Phase 1 (AES/SHA/DH Group 2), IPsec Phase 2 (ESP-AES-SHA)

Routing: Static Routing with Default Gateways

 Troubleshooting & Resolution Log
During the deployment of this VPN, several common Layer 1 and Layer 3 issues were identified and resolved:

1. Issue: "MM_NO_STATE" in ISAKMP SA
Symptoms: Tunnel failed to establish; show crypto isakmp sa displayed MM_NO_STATE.

Cause: This was a Phase 1 Handshake failure. The two routers could not agree on the security policy (mismatched Hashing/Encryption) or had a typo in the Pre-Shared Key (PSK).

Resolution: Standardized the ISAKMP policy to use AES-128 and SHA on both ends and re-applied the crypto isakmp key to ensure matching addresses and passwords.

2. Issue: "Incomplete State" Warning on Crypto Map
Symptoms: Site B router refused to encrypt traffic; show crypto map reported an "incomplete state".

Cause: The Crypto Map was missing its link to the Access Control List (ACL), meaning it didn't know which traffic was "interesting" and required protection.

Resolution: Executed the match address 101 command within the Crypto Map configuration to bind the ACL to the VPN policy.

3. Issue: Initial Packet Loss (25%)
Symptoms: The first ping from PC-A to Server-B failed, while subsequent pings succeeded.

Explanation: This is expected behavior for a "lazy" VPN tunnel. The first packet triggers the IKE negotiation process; since the tunnel isn't fully built until the handshake completes, the initial packet times out.

Verification: Once established, the tunnel remains in QM_IDLE state for 3600 seconds (default), allowing for 0% packet loss on further communication.
