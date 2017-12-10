# Godot Engine

Godot is a 2D/3D Game Engine with a built-in editor. A lot of the editing is done visually (think Photoshop/Blender meet .NET), helped by a custom programming language, GDScript, that resembles Python.


## Comprehensive Documentation

http://docs.godotengine.org/en/stable/


## Scenes and Nodes

A game is composed of scenes.

Each scene has a root node (e.g.: Node2D). Each node can have child nodes (Sprites, Labels, Buttons, etc).

You can instantiate scenes as parts of another scene. If you design a scene for an enemy, you can create multiple instances of it to have a lot of enemies on screen.


## Scripts

Nodes can have scripts that run in an event-driven fashion (events are called signals in Godot).

### Common lifecycle hooks

```gdscript
_ready() # When the node and its children are injected in the scene
```

### Handle button press

```gdscript
func _on_button_pressed():
    get_node("Label").set_text("HELLO!")

func _ready():
    get_node("Button").connect("pressed", self, "_on_button_pressed")
```
