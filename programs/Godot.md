# Godot Engine

Godot is a 2D/3D Game Engine with a built-in editor. A lot of the editing is done visually (think Photoshop/Blender meet .NET), helped by a custom programming language, GDScript, that resembles Python.


## Comprehensive Documentation

http://docs.godotengine.org/en/stable/


## Scenes and Nodes

A game is composed of scenes.

Each scene has a root node (e.g.: Node2D). Each node can have child nodes (Sprites, Labels, Buttons, etc).

You can instantiate scenes as parts of another scene. If you design a scene for an enemy, you can create multiple instances of it to have a lot of enemies on screen.

Nodes can be part of a Group. This helps when you want to execute code in multiple Nodes, e.g., alerting all enemies when the player is discovered by one of them.


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
    # When set_process() is enabled, this function is called every frame.
    pass

func _fixed_process(delta):
    # When set_fixed_process() is enabled, this is called every physics
    # frame.
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

Be careful with handler function arguments, they must match the signal exactly or they won't be called.

You can also use the visual editor to connect Nodes (in the Inspector > Node section), a plug icon will be displayed next to the Script icon.

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
func _ready():
    set_process(true) # Turn on frame processing

func _process(delta):
    if Input.is_action_pressed("ui_tap"):
        print("tap")
```


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
