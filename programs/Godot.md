# Godot Engine

Godot is a 2D/3D Game Engine with a built-in editor. A lot of the editing is done visually (think Photoshop/Blender meet .NET), helped by a custom programming language, GDScript, that resembles Python.


## Comprehensive Documentation

http://docs.godotengine.org/


## Scenes and Nodes

A game is composed of scenes.

Each scene has a root node (e.g.: Node2D). Each node can have child nodes (Sprites, Labels, Buttons, etc).

You can instantiate scenes as parts of another scene. If you design a scene for an enemy, you can create multiple instances of it to have a lot of enemies on screen.

Nodes can be part of a Group. This helps when you want to execute code in multiple Nodes, e.g., alerting all enemies when the player is discovered by one of them.

## Images

Images are imported to the hidden `.import` folder. To change their import settings, click them in the `File System` tab, choose the `Import` tab (next to `Scene`), change settings and then click **Reimport**.


## Physics

Remember to change the Default Gravity in the project's settings (Physics or Physics 2d)

- StaticBody: not affected by physics but affects others
- KinematicBody: not affected by physics, but can be manipulated by code or animations (e.g. moving platforms)
- RigidBody: affected by gravity and physics

Inside the body, you can have a Sprite to render and a CollisionShape to define its physics bounds.

### No friction

To disable friction in a body, set the Friction and Damp Override to 0.

### No rotation

To disable rotation in a body, set its Mode to Character.

### Collision masks and layers

You can set Player to Layer/Mask 1, Powerup to Layer/Mask 2 and Platform to Layers/Masks 1 + 2. The player and powerup will collide with platform but not with each other.


## TileSets

Create a TileSet:

1. Turn on Project Settings > Display > Use 2d Pixel Snap
2. Create a TileSet Scene
3. Edit > Show Grid, Edit > Use Pixel Snap
4. Add a Sprite for each tile (specify the Region Rect for each individual Sprite to load different tiles from the same image)
5. Name each Sprite correctly and uniquely
6. Add collision to relevant tiles with StaticBody2D and CollisionPolygon2D/CollisionShape2D
7. Scene > Convert To > TileSet (.tres)

Use a TileSet:

1. Create a TileMap Node
2. Load the TileSet
3. Set the Cell Size to the correct value
4. Lock the TileSet Node to avoid moving it unintentionally (lock icon in the toolbar)


## Scripts

Nodes can have scripts that run in an event-driven fashion (events are called signals in Godot).

A Script defines a class that extends the Node's base class.

**Conventions**:

- Nodes are `CamelCase`.
- Functions and variables are `snake_case`.
- Private class members start with underscore `_`.
- Signals are in the past tense `something_happened`.

### Common lifecycle hooks

```gdscript
func _enter_tree():
    # When the node enters the _Scene Tree_, it becomes active
    # and  this function is called. Children nodes have not entered
    # the active scene yet. In general, it's better to use _ready()
    # for most cases.
    pass

func _ready():
    # This function is called after _enter_tree, but it ensures
    # that all children nodes have also entered the _Scene Tree_,
    # and became active.
    pass

func _exit_tree():
    # When the node exits the _Scene Tree_, this function is called.
    # Children nodes have all exited the _Scene Tree_ at this point
    # and all became inactive.
    pass

func _process(delta):
    # This function is called every frame.
    pass

func _physics_process(delta):
    # This is called every physics frame.
    pass

func _paused():
    # Called when game is paused. After this call, the node will not receive
    # any more process callbacks.
    pass

func _unpaused():
    # Called when game is unpaused.
    pass
```

### Signals

To handle a Node's signal, do:

```gdscript
func _ready():
    connect("signal_name", self, "_my_handler")

func _my_handler()
    # Handle signal here
```

You can connect to children Nodes' signals by doing `get_node("...").connect(...)`.
You can get other Nodes by doing `get_tree().get_root().get_node("...")`.

`$<node_name>` is syntax sugar for `get_node(<node_name>)`.

Be careful with handler function arguments, they must match the signal exactly or they won't be called.

You can also use the visual editor to connect Nodes (in the Inspector > Node section), a plug icon will be displayed next to the Script icon.

#### Trigger custom signals

```gdscript
signal my_signal(param1, param2)
...
emit_signal("my_signal", "value1", "value2")
```

#### Handle button press

```gdscript
func _on_button_pressed():
    get_node("Label").set_text("HELLO!")  # Label is a child node named "Label"

func _ready():
    get_node("Button").connect("pressed", self, "_on_button_pressed")  # Button is a child node named "Button"
```

### Adding and deleting Nodes/Scenes

Nodes:

```gdscript
s = Sprite.new() # create a new sprite!
add_child(s) # add it as a child of this node

s.queue_free() # remove the node and delete it while nothing is happening
```


Scenes:

```gdscript
var scene = preload("res://myscene.scn") # will load when parsing the script
# Alternative:
var scene = load("res://myscene.scn") # will load when the script is instanced

var node = scene.instance()
add_child(node)

node.queue_free() # remove the node and delete it while nothing is happening
```

## Input Map

To add input actions, go to Project Settings > Input Map. E.g.: add an action called `ui_tap`.

To listen to it, do the following in a Script:

```gdscript
func _process(delta):
    if Input.is_action_pressed("ui_tap"):
        print("tap")
```
## HUD

To create a HUD, add a CanvasLayer Node.

To anchor a Control to somewhere on the screen, set its Control > Anchors. E.g.: set Left, Right = End, and Top, Bottom = Begin to anchor a Control to the top right corner of the screen.

## Parallax Background

Add the following nodes: ParallaxBackground > ParallaxLayer > Sprite

In the ParallaxLayer, set the Motion Scale to scroll the Sprite at a different rate than the movement.

If needed, repeat the background by enabling Region in the Sprite (to a region bigger than the image) and add Repeat to the Texture Flags (inside the Sprite's texture).

## Setup Export to Android

1. Download Export Templates from https://godotengine.org/download
2. Install Export Templates in Godot > Settings > Install Export Templates
3. Configure Android Export (Settings > Editor Settings):
   - Adb: `/home/<user>/Android/Sdk/platform-tools/adb`
   - Jarsigner: `/opt/android-studio/jre/bin/jarsigner`
   - Debug Keystore: `/home/<user>/.android/debug.keystore`
