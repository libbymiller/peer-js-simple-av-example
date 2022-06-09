room-audio-multipeer-with-presence has a room to provide data about who is connected but which itself is only 
connected via data. Each peer calls all the others when they get a list from the room of who is connected.

index.html maintains a list of when peers were last seen.

They use ../peer.js which is very slightly modified version to notify on disconnects (line 7972)

