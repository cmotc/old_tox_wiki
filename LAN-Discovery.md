LAN dicovery packets are sent to the broadcast address of the LAN.

LAN discovery packet:

[byte with value: 32 (10 in hex)][32 byte client_id of sender]

As soon as a client recieves a discovery packet he checks if it came from a LAN ip.

If the packet came from a LAN ip, the client sends a get nodes request to the peer that sent the packet.
