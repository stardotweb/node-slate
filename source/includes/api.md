# Programming Interface

## Auth

```javascript
import {Auth} from "livehelp";

Auth.authorize("<credentials>")
    .then(
        session => { console.log("I am authenticated!", session); }
        error => { console.log("No gravy!", error); }
    )
;
```

`authorize("<credentials>")`

Returns a `Promise` which resolves to the `Session` instance or is rejected with an error

Parameter | Description
--------- | -----------
credentials | -


## Session
```javascript
import {Auth} from "livehelp";

Auth.authorize("<credentials>")
    .then(session => {
        const rooms = session.listRooms();
        console.log("Found " + rooms.length + " rooms ...");

        const room = session.getRoom(rooms[0].id);
        console.log("First Room is " + room.displayName);

        console.log("Creating 'My Special Room' ... ");
        const myRoom = session.createRoom({
            displayName: "My Special Room"
        });

        session.invalidate().then(response => {
            console.log("I'm outta here!");
        });
    })
;
```

`listRooms()`

Returns an array of `Room` instances

<br>

`getRoom(roomId)`

Returns a specific `Room` instance

Parameter | Description
--------- | -----------
roomId | ID of the room

<br>

`createRoom(config)`

Returns the newly created `Room` instance

Parameter | Description
--------- | -----------
config | Configuration object that is passed to the constructor of `Room` 

<br>

`invalidate()`

Returns a `Promise` which resolves when the `Session` is invalidated at the server


## Room
```javascript
import {Auth} from "livehelp";

Auth.authorize("<credentials>")
    .then(session => {
        console.log("Creating a room with everything except chat ... ");
        const myRoom = session.createRoom({
            type: {
                video: true,
                whiteboard: true,
                chat: false
            },
            displayName: "My Special Room",
            inviteOnly: true,
            private: true
        }); // returns room with ID, say, "abcde-abcde-abcde-abcde-abcde"

        myRoom.setPrivate(false);
        myRoom.setInviteOnly(false);

        const users = myRoom.listUsers();
        console.log("Currently, there are " + users.length + " users here...");
    })
;

Auth.authorize("<credentials>")
    .then(session => {
        // A user enters the previously created room
        const thatRoom = session.getRoom("abcde-abcde-abcde-abcde-abcde")
            .join()
            .then(collab => {
                console.log("Joined the room !!");
            });
    })
;

```

`constructor(config)`

Create a new `Room` instance

Config Property | Default Value | Description
--------------- | ------------- | -----------
type | {video: true} | Object with flags representing `video`, `chat`, `whiteboard`
displayName | - | A human-readable name for the room
inviteOnly | false | Flag this room as invite-only.
private | false | Flag this room as private

<br>

`setPrivacy(privacy)`

Set the privacy mode of the room.

Parameter | Description
--------- | -----------
privacy | Setting to `true` will hide this room from `Session.getRooms()` listing

<br>

`setInviteOnly(invitation)`

Set the invite-only mode of the room.

Parameter | Description
--------- | -----------
invitation | Setting to `true`, the creator of the Room will need to approve all admissions into the room

<br>

`getType()`

Returns an object with flags representing `audio`, `video`, `chat` and `whiteboard` mode availability for this room

<br>

`listUsers()`

Returns an array of `User` instances in this room

<br>

`join()`

Returns a `Promise` which resolves if the user was able to join the room or is rejected with an error

<br>

`leave()`

Returns a `Promise` which resolves if the user was able to leave the room

## Collab
```javascript
import {Auth} from "livehelp";

Auth.authorize("<credentials>")
    .then(session => {
        // Entering a room with everything ...
        const thatRoom = session.getRoom("abcde-abcde-abcde-abcde-abcde")
            .join()
            .then(collab => {
                collab.audio.mute();
                collab.video.freezeFrame();
                collab.whiteboard.annotate();
                collab.chat.sendMesaage();
            });
    })
;
```

`audio`

The audio-collaboration specific object for this `Room`. An instance of `AudioCollab`

<br>

`video`

The video-collaboration specific object for this `Room`. An instance of `VideoCollab`

<br>

`chat`

The chat-collaboration specific object for this `Room`. An instance of `ChatCollab`

<br>

`whiteboard`

The whiteboard-collaboration specific object for this `Room`. An instance of `WhiteboardCollab`

<br>


## WhiteboardCollab
```javascript
import {Auth} from "livehelp";

Auth.authorize("<credentials>")
    .then(session => {
        // Entering a room with everything ...
        const thatRoom = session.getRoom("abcde-abcde-abcde-abcde-abcde")
            .join()
            .then(collab => {
                let whiteboard = collab.whiteboard;

                console.log("Drawing a line ...");
                whiteboard.annotate('line', {
                    "x1": 14, "y1": 18,
                    "x2": 18, "y2": 14,
                    "strokeWidth": 5,
                    "strokeColor": "red",
                    "fillColor": "red"
                });

                whiteboard.on("annotation_add", (body, message) => {
                    console.log(body.type + " annotation added by user " + message.origin);
                })
            });
    })
;
```

`annotate(annotation)`

Create and broadcast an annotation.

<br>

`on(eventName, handler)`

Listen to a specific type of broadcast message for the whiteboard session of this room

Parameter | Description
--------- | -----------
eventName | Name of the broadcast type to listen for
handler | Handler function reference

<br>

`listen(handler)`

Listen to any and all broadcast messages for the whiteboard session of this room

Parameter | Description
--------- | -----------
eventName | Name of the event 

<br>

