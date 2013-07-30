A couple of ideas posted in the threads
=======================================

Presence (online/offline)
--------------------------
- A user's id being present with a valid signature in the DHT implies they have been online recently. A ping to the user would confirm this, as well as notifying them that you are online. If both friends in a pair actively search for the other then disruption due to lag in inserting records will be mimimised

Username (nick) changes
-----------------------
- When a user wants to change their nick they are free to do so, as the nick is not cryptographically significant. The nick change could be shared in-band with the chat (ie, each message in the format "username - message") or out of band, perhaps in the DHT values or in the ping messages.

File send
---------

- I see a file being sent as ultimately the same as an audio stream or a video stream being sent, they could use the same protocol parts. The client would handle what to do with the data whether it is a stream of media data or a stream of b64'd file. Both would require explicit verification from the other participant. This would also allow sharing of streamed files, eg incomplete downloads.


...


- ability to log in with username and password
    possible implementation: store keys in a server and use user/pass to retrieve keys from server transparently
   >too centralized
- looking up people and adding as contacts, etc by name or username
    possible implementation: store pubkey-hash/info correspondences in a public directory, integrate access into client
- portable contact list/profile/other account data
    just store it along with keys in aforementioned server; cost of storage could rise... potential problem
-centralized servers are points of failure.  But we could allow people to merge identities, which would allow transmission.  Make a temp id to transmit to, transmit your private and public key, your contacts, etc.
- POSSIBLY interfacing with regular phones. probably not possible, but plebs might complain (THIS IS EXPENSIVE (hence 'probably not possible')) <3

IMPORTANT: Release two UIs, an "advanced" one (so powerusers don't feel like the UI is in their way) and a more "intuitive" one (so average users get started quickly)

de geso
- > I would suggest a "Advanced options" where the advanced users can rejoice with all kinds of options (and it doesn't frighten the newbs, since it's not shown by default). Also, 2 UIs would be chaos to maintain. 