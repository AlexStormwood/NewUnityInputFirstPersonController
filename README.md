# New Unity Input First Person Controller
 Cross-platform first-person controller demo, for M&KB and gamepad.

# Project & Package Versions

* Unity 2020.1.1f1
* Input System 1.0.0

# Project Explanation

This is a reeeeeeeeally basic first-person character controller that can simply move, turn around, and look around. It has support for "two" control schemes: keyboard & mouse, and gamepad.

Because "gamepad" is generic, this technically works for Xbox, Playstation and Nintendo Switch without general modification anyway (you would still have to do controller detection in those platforms SDKs when relevant but that's beyond scope of this demo). It's good practice to make specific control schemes for those platforms when possible - this demo does not do that, and instead sticks to the generic gamepad.

This demo is built to showcase the workflow of the new Unity Input System, specifically its Player Input component.

<img src="/ReadmeAssets/PlayerInputComponent.png" style="zoom:125%;" />

The demo comes with a pre-made Input Action Asset, already set up with 2 control schemes and associated bindings.

* The "Keyboard & Mouse" control scheme requires both keyboard & mouse devices to be active, and relates to bindings like WASD keys and mouse pointer deltas.
* The "GenericGamepad" control scheme requires any 'gamepad' controller to be active. This could be a Nintendo Switch controller, an Xbox One controller, a Playstation 4 controller, etc etc. Unity's definition of 'gamepad' can be found [here](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/Gamepad.html).

<img src="/ReadmeAssets/InputActionSchemeExample002.png" style="zoom:125%;" />

<img src="/ReadmeAssets/ControlSchemeExample002.png" alt="ControlSchemeExample002" style="zoom:150%;" />

By tying into the PlayerControls Input Action asset, the game can detect & switch between control schemes automatically. The control schemes are set up to require a gamepad for the GenericGamepad scheme, and require both a keyboard and mouse for the Keyboard&Mouse scheme. When inputs are detected on devices required by a different scheme, the scheme used by the PlayerInput component changes to match the most-recently-active scheme. You don't have to code this yourself - it's automatic switching!

The player itself is really rudimentary - it does not have ground detection, jumping, or anything production-ready beyond moving & looking around. The player object structure is this:

<img src="/ReadmeAssets/HierarchyOfPlayer.png" style="zoom:150%;" />

And yes, the "FirstPersonPlayerContainer" object is not meant to move or rotate. The idea here is that you can easily detach the player character from the player data - if the player dies, you could still have things like account data tied to the container rather than the gameobject that you're about to destroy & remove from the RAM. Better management of your player this way. It's probably a bit beyond what this demo should be showing but I think it's still worth seeing.

The idea is that you interact with the player character in several different sections:

* move the Body to move the player around
* rotate the Body left & right to turn the player around & look around
* rotate the Head up & down to look around

Because you're moving & rotating different parts at different times, the camera's view doesn't go all crazy. It's exactly what you'd expect from character controllers like the old Unity Standard Assets First Person Controller.

So, how does the New Input System interact with the player to make it move & turn & look? I made a script for that:

<img src="/ReadmeAssets/InspectorOfPlayer.png" style="zoom:150%;" />

The BasicPlayerController.cs file should have some nice, readable code comments for you. Check it out here: [BasicPlayerController.cs](https://github.com/AlexHolderDeveloper/NewUnityInputFirstPersonController/blob/master/Assets/Scripts/Gameplay/BasicPlayerController.cs)

But the general idea is this:

* The MoveInputs and LookInputs vector2 variables are tied to the PlayerInput component. When inputs are received in the PlayerInput component, the data is fed through to some functions in the BasicPlayerController component that updates those vector2 variables in the BasicPlayerController script.
* In the BasicPlayerController's Update function, the player turning & looking logic is handled. 
* In the BasicPlayerController's FixedUpdate function, the player movement logic is handled. It's handled here instead of in Update because it is using a Rigidbody, and it's bad for performance to work with Rigidbodies on every frame of the game.





















And yes, this means the project ***does*** have nice mouse-look controls out-of-the-box. 



