# Room Design Template Instructions
_Note: In Somniphobia, I was in charge of a large part of designing and implementing procedural level generation into the game. To allow the designers to work on individual rooms
of the game's labyrinth as well as test a given room separate from the entire generated level, we utilized Unreal's level streaming to allow each room to be its own level that then
has its information fed into a couple of data tables to then automatically be integrated into the procedural generation algorithm. I wanted the designers to be able to create new
rooms and implement them into the algorithm without having to ask me how to do so, so I put together this document to teach them how._

## Scale
The scale of a room determines the smallest rectangle of tiles on the grid in which the room can be contained. These are the height and the width, stored within an IntPoint named “Scale.” The X field should contain the height, and the Y field should contain the width. 

<img width="546" height="346" alt="scale" src="https://github.com/user-attachments/assets/da640f68-74e9-4354-8e83-84e9ad05c50e" />

## Shape Archetype
The shape archetype represents however many tiles a designer wants this room specifically to take up. It doesn't matter if the room perfectly fits those tiles, it just matters what the intention for it to be represented on the graph is.

A shape archetype is a series of 1s and 0s in a grid shape, with 1s representing filled tiles and 0s representing empty ones. The method with which one chooses to define its shape doesn't matter, so long as the room takes up no more than 1 tile's worth of physical space more than its shape architecture would imply. Below are some examples I've given to show off what could be decided with the archetypes. 

<img width="624" height="468" alt="shapearchetype" src="https://github.com/user-attachments/assets/7a2e174e-0aa0-4072-8954-904816bea31e" />

If a room is physically larger than its shape archetype, it may overlap with another room, but this won't be represented on the graph, and as long as the spatial inconsistency is smaller than a single tile, the player will almost certainly never notice.

The specific dimensions of a single tile are wholly arbitrary and will be left up to the discretion of design. However, the size of a tile must be consistent between all rooms, so deciding a universal tile size is essential. Also, a tile must be square, meaning it must be equilateral.

<img width="369" height="351" alt="shapearchetype2" src="https://github.com/user-attachments/assets/397197e4-5d80-44a7-b384-5e6db44401c9" />

## Key ID
This is an integer representing an ID for the key/lock associated with this room. If the number is positive, it means the room contains a key of the type associated with the given value. If the number is negative, it means the room contains a door requiring the key associated with the given absolute value to unlock. As of now, a room may only contain one lock or one key.

## Doors
Each room will contain an array of doors. Doors are their own structure, containing their own set of necessary data.
	
  - Target Door: This is the door which the door will lead to when activated.
	
  - Local Position: These coordinates represent the point (row, col) which the door is located within the room. A door does not necessarily need to be exactly within a tile, though keep in mind the map grid will behave as if it is, causing a minor spatial inconsistency between the room and the door that the player is highly unlikely to ever notice. 
	
    <img width="483" height="310" alt="doors" src="https://github.com/user-attachments/assets/15893ae2-9cfd-4e4a-b183-ba9d5b476fcd" />

  - Is Locked: This is a simple boolean specifying whether the door is locked. The locked door will be assumed to require the key associated with the positive equivalent of its KeyID.
	
  - Orientation: This is an enumerator specifying the orientation of the door according to the cardinal directions. Simply label the door with the cardinal direction it faces in its default rotation.
  
    <img width="435" height="337" alt="orientation" src="https://github.com/user-attachments/assets/844eb163-3b8e-441c-9ded-f9b9235e059d" />


## Implementing a Room’s Data
When a room is finished being created, its data must be added to the Room List. This is merely a data table in the editor containing the data needed to process a room within the generation algorithm. The name for the file containing the list is called DT_RoomList, and is located in All > Content > Data > DataTables.

<img width="776" height="269" alt="roomdata1" src="https://github.com/user-attachments/assets/c858dfb5-f219-47a0-9d57-0a61ea4c4535" />

Within the Room List, you should see a button labeled “Add.” Press this button to add a room to the list.

<img width="673" height="305" alt="roomdata2" src="https://github.com/user-attachments/assets/d323ecc0-e097-4d59-a09a-9823e861a83f" />

When you do so, a new entry should appear in the table with a name like “NewRow.” Right-clicking on this entry should allow renaming it. The name should be identical to the name of the room’s associated level. Double-clicking on the entry will open the data stored within it to appear in the Row Editor. Fill in the information here according to the above instructions. Rotation is a field meant for us and is not necessary to fill in.

<img width="828" height="347" alt="roomdata3" src="https://github.com/user-attachments/assets/f470fec7-0692-4b49-90a6-c976d0c302de" />


## KeyData & KeyList

After the room has been added to the RoomList, one last step must be taken. In the same folder as the RoomList is another Data Table named “KeyList.” Similarly to the RoomList, this is a list of hard-coded keys for the algorithm to recognize by their IDs. For each unique KeyID added, a new row must be added to this Table with the following data:
	
  - ID: This is the int which represents the key and its associated lock. It should be identical to the ID given in any room it is included in.
	
  - Name: This is the name of the key. This will become the name of the Key Actor which will be instantiated within the room.
