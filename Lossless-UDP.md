Lossless UDP:
A network protocol over UDP to transfer data without any loss.

Problems with conventional UDP:
-Packet loss.
-Packet mixup.
-No flow control.

Draft of Proposed protocol
==========================

Connection example
-----------------

Alice wants to connect to Bob (Connection handshake):
Alice generates a random 4 byte number.
Alice puts it in the handshake packet (handshake_id1).
Alice starts sending handshake packets to Bob (send 10 packets over 5 seconds if no response connection fails.)
Bob receives the packet.
Bob copies the handshake packet he got from Alice but concatenates a random 4 byte number to it (handshake_id2)
Alice receives the packet, checks if handshake_id1 matches the one she sent.
If it does she starts sending SYNC packets with sent_packetnum = handshake_id2 and recv_packetnum = handshake_id1.
Bob receives the packet, 
if (handshake_id2) matches Bob adds Alice to his connections list and starts sending SYNC packets to Alice.
Alice receives the SYNC packets and starts sending them too.
Connection is now established.

Alice wants to send packets to Bob:
Alice sends packets to Bob.
If Bob receives the packets, his next SYNC packets reflect it.
Alice receives the SYNC packets which confirms that Bob received them
Alice clears the packets from her buffer.

If Bob does not receive any packets, but receives a SYNC packet with a larger sent_packetnum than his recv_packetnum:
Bob's SYNC packets become request packets.
The packet id of the missing packet(s) are added to the end of the SYNC packet.
Alice resends the packets whose numbers appear at the end of the request packet.

If Bob receives some packets but some are missing
Bob's SYNC packets become request packets.
The packet id of the missing packet(s) are added to the end of the SYNC packet.
Alice resends the packets whose numbers appear at the end of the request packet.

Alice and Bob disconnect suddenly:
Alice stops receiving SYNC packets from Bob.
Bob stops receiving SYNC packets from Alice.
Connection times out if no data is received for 5 seconds.               

Packet handling
---------------

The client receives a SYNC packet:
He checks if the packet is valid(sent_packetnum and counter make sense)
If the packet is valid:
If the client was currently trying to establish a connection.
If the handshake was done:
Start sending SYNC packets.
Add +1 to the counter of the SYNC packets we send.
Check to see if any packets were dropped when sending them to us (compare sent_packetnum with the number of packets we received.)
If some were, The next SYNC packets we send will be request packets requesting the missing packets.
Check if the other client received what we sent him (compare recv_packetnum with the number of packets we sent) and clear them from the buffer.
If the packet is a request packet.
Send the packets he requested.



The client receives a Connection handshake packet(not initiating connection):
If handshake_id2 is zero:
use the handshake_id1 in the packet as the handshake_id2 of the response.
use our random handshake_id1 as handshake_id1.
send the response packet.



The client receives a Connection handshake packet(initiating connection):
check if handshake_id1 matches what we sent.
If it does start sending SYNC packets with sent_packetnum as handshake_id2

The client receives a data packet:
Put the packet in the proper place in the receive buffer using packet_num as the reference.
If all the previous packets have been received correctly set recv_packetnum equal to packet_num



Keep track of the percent of packets dropped, if it is too high, lower the send rate. If it is low, increase it.

Have a send packets buffer that we can write to and a received packets buffer that we can read from.




Protocol:
Connection handshake packets:
```
[byte with value: 16 (10 in hex)][4 byte (handshake_id1)][4 bytes (handshake_id2)]
```

SYNC packets:
```
[byte with value: 17 (11 in hex)][byte (counter)][4 byte (recv_packetnum)][4 byte (sent_packetnum)][sequence of requested packet ids[4 bytes each](not present in keep alive)]
```


data packets:
```
[byte with value: 18 (12 in hex)][4 byte (packet_num)][data (arbitrary size)]
```