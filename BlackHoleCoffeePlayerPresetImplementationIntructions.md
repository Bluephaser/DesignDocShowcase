# Player Preset Implementation Instructions

_Context: For the prototyping phase of Black Hole Coffee, I wanted to make sure that designers can easily switch between different presets for player movement "stats" on the fly
during play. In code, the way I accomplished this was by integrating paths to Godot resources on the player character, and created a child node that manages switching between them.
For even easier implementation, I made the code of the preset switcher read the number at the end of the preset resource's name and link that to an input event connected to the
equivalent number key so that once a new preset is made and its path is added to the player, it can be switched to in-game with no further additions to code. I then made this
document to explain to designers how to implement new presets with the goal that designers never need to be roadblocked by tech when prototyping._

# 

For easy prototyping, designers should avoid doing testing by changing player stats by hand. Instead, utilizing Godot Resources, you can make a preset list of stats for the player to toggle between in-game. Resources are a form of data-driven class, much like ScriptableObjects in Unity.

To start, navigate to the following path:  res://Assets/Resources/Characters/Player/

<img width="528" height="372" alt="image" src="https://github.com/user-attachments/assets/77fde629-d66b-4c4f-832e-81d8754b28ae" />

Right click on the Player folder, and select Create New → Resource. 

<img width="975" height="348" alt="image" src="https://github.com/user-attachments/assets/5d42c65e-188b-4826-a76e-9fb9a6b008da" />

Search for PlayerPreset in the Resource options and double-click it. Then, name your file PlayerPresetX.tres, where X is the next number that hasn’t been taken by one of the files. The player can have a maximum of 9 presets.   

<img width="504" height="253" alt="image" src="https://github.com/user-attachments/assets/aa512308-ee2d-4702-852a-2370e01decae" />

<img width="863" height="481" alt="image" src="https://github.com/user-attachments/assets/51420ca8-e11f-4c66-ab4f-3dcb2cee706a" />

Before you start altering the variables on your resource, make sure that you add its path to the player character. To do this, right click on your resource and click “Copy Path.” Then, open the player scene and find the PresetPaths array in its editor fields. Add a new entry to this array, then paste the copied path into that entry.

<img width="211" height="268" alt="image" src="https://github.com/user-attachments/assets/36a6cad0-f5a7-47a3-9d21-cbc5d57a2352" />

To edit a resource, double-click it. You should see its fields appear on the right-hand side of the editor.

<img width="226" height="297" alt="image" src="https://github.com/user-attachments/assets/f6e6f1e5-7838-4aa5-8036-73f46208b596" />

For more details on how these variables actually work, check the comments in Character.cs and Player.cs on the Export variables.

<img width="975" height="307" alt="image" src="https://github.com/user-attachments/assets/6746acb3-71f9-4a12-9487-94417c0e08ac" />

<img width="975" height="323" alt="image" src="https://github.com/user-attachments/assets/c4006dc1-390c-4948-8c0d-6b577a20029b" />

Once you’ve finished all these steps, you should be able to switch between states just by pressing the associated number key (For PlayerPreset1, press the 1 key). If you have any more questions, don’t hesitate to ask me (Liam/Berb) for help.
