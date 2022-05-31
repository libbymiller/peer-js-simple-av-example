Here are various experiments.

All except room-audio-multipeer-with-presence only deal with two peers.

room-audio-multipeer-with-presence has a room to provide data about who is connected but which itself is only 
connected via data. Each peer calls all the others when they get a list from the room of who is conneceted.

index.html maintains a list of when peers were last seen.

peer.js is very slightly modified version to notify on disconnects (line 7972)

