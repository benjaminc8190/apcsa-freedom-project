# Tool Learning Log

## Tool: **Unity**

## Project: **The New Settlement**

---

### 10/02/24: [Unity tutorial for Beginners](https://www.youtube.com/watch?v=XtQMytORBmM)
* Unity User Interface
  * Top left: hierarchy- Contains what happens in the scene, drag sprites up to create game object
  * Bottom: project- game elements such as sprites and scripts, usually imorted images
  * Top middle: scene- Basically the preview for what happens in the scene, select game view to see how the game will look like from user perspective
  * Left: Inspector- Manipulating game objects to perform actions and name the objects

### 10/14/24: [W3 Schools](https://www.w3schools.com/cs/index.php)
* Created a codespace by following the instructions on Get Started section and got this:
```C#
namespace HelloWorld
{
    internal class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```
Tried to replicate what w3school shows by copying `Console.WriteLine(Value);` but showed error and told me to declare a value to output it:
```C#
namespace HelloWorld
{
    internal class Program
    {
        private const string Value = "Hi";

        static void Main(string[] args)
        {
            Console.WriteLine(Value);
        }
    }
}
```
Notes so far:
* `Console.WriteLine();` is like `System.out.println();`
* C# is very similar to java since it is object-oriented

### 10/26/24: [Godot Showcase](https://godotengine.org/showcase/), [First game in Godot](https://www.youtube.com/watch?v=iwGVLiFL-Lw)
* Pros of switching to Godot:
 * Godot is very good for small companies or solo dev while Unity is mostly for game industry and big companies
 * Most people find Unity annoying while Godot is a lot more easier and more fun to code in
 * Skills are easily transferrable
 * everybody is using it so we can ask when need help
 * tutorials are much more recent
![Godot](../img/godot-ui.png)
![Unity](../img/unity-ui.png)

### 11/7/24: [Freecodecamp course](https://www.youtube.com/watch?v=S8lMTwSRoRg&t=923s)

The following basically serves as a menu to either exit the game or play the game(it's supposed to bring you to the World file scene which shows nothing so far):
![menu and script](../img/godot_scene_script_and_preview.png)

This is my less developed game so far:

```GDscript
extends Node2D


func _on_quit_pressed() -> void:
	get_tree().quit() # the tree is everything(basically a parent of root which is the parent of Main)


func _on_play_pressed() -> void:
	get_tree().change_scene_to_file("res://world.tscn") # Goes to the world scene
```

I will be trying to find assets and already-drawn arts to move as sprites and continue testing them out.

### 11/7/24: [Freecodecamp course(continue)](https://www.youtube.com/watch?v=S8lMTwSRoRg&t=923s) & [Assets](https://ansimuz.itch.io/sunny-land-pixel-game-art)

Default template:

```GDscript
extends CharacterBody2D


const SPEED = 300.0
const JUMP_VELOCITY = -400.0


func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var direction := Input.get_axis("ui_left", "ui_right")
	if direction:
		velocity.x = direction * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)

	move_and_slide()
```
Note to self: make sure the files are children of the proper parent so that they do not act together, the floor was a child of Player so it had the same physics and therefore it fell due to the sprite(Player) falling physics. Here is the correct parent-child relationship to keep the static body from falling with the sprite:
![menu and script](../img/animation-files.png)

### 12/6/24: [Freecodecamp course(continue with adding animations:)](https://www.youtube.com/watch?v=S8lMTwSRoRg&t=923s)

```GDscript
# Animation
@onready var anim =get_node("AnimatedSprite2D") #onready is for accesing the variable on runtime
func _ready():
	anim.play("Idle")

# Game Physics
func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var direction := Input.get_axis("ui_left", "ui_right")
	if direction:
		velocity.x = direction * SPEED
		anim.play("Run")
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)
		anim.play("Idle")
```
However, I will not be able to implement the jump animation into this code because the sprite is simutaneosly jumping and running at the same time. Thus, I must replace the `"AnimatedSprite2D"` from line 2 with `"AnimationPlayer"` which is a different object for animating the sprite. Another alternative is using the `AnimationTree` object

```GDscript
# Animation
@onready var anim =get_node("AnimationPlayer") #onready is for accesing the variable on runtime
func _ready():
	anim.play("Idle")

# Game Physics
func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta
	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY
		anim.play("Jump")

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var direction := Input.get_axis("ui_left", "ui_right")
	if direction == -1:
		get_node("AnimatedSprite2D").flip_h = true
	elif direction == 1: #else if
		get_node("AnimatedSprite2D").flip_h = false
	if direction:
		velocity.x = direction * SPEED
		if velocity.x == 0:
			anim.play("Run")
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)
		if velocity.y == 0:
			anim.play("Idle") #proper indents
	if velocity.y > 0:
		anim.play("Fall")
```
The newly added logic statements allow the sprite to look in the directions it is moving and features the jump and fall animation
However, the jump animation is still not rendering so I must use the logic between the velocity and when the sprite hits the floor which makes the velocity of y 0.

Here is the animation frames for the idle animation:
![menu and script](../img/godot-animation.png)

## 12/3/24 [Merge Conflict testing](https://www.youtube.com/watch?v=fZ-CJIYPFMI&t=215s)
Yu, William, and I tried to provoke merge conflicts and attempted to start the collaboration project using Github Desktop. 
A merge conflict happened when we had one person add objects and another person without objects commit at nearly the same time. The person who pushed last recieved this:
```GDscript
[gd_scene load_steps=3 format=3 uid="uid://rcd7vc7mj8e2"]

[ext_resource type="PackedScene" uid="uid://bhglro57ltxjk" path="res://character_body_2d.tscn" id="1_oivte"]
[ext_resource type="Script" path="res://movement.gd" id="2_8siem"]

[node name="Node2D" type="Node2D"]

<<<<<<< HEAD
[node name="CollisionShape2D4" type="CollisionShape2D" parent="."]
position = Vector2(407.313, 226)
shape = SubResource("RectangleShape2D_cbba5")

[node name="CharacterBody2D" parent="CollisionShape2D4" instance=ExtResource("1_oivte")]
position = Vector2(-90.3125, -89)

[node name="CollisionShape2D" type="CollisionShape2D" parent="."]
position = Vector2(580, 860)
shape = SubResource("RectangleShape2D_f4ko0")

[node name="CollisionShape2D2" type="CollisionShape2D" parent="."]
position = Vector2(-162, 326)
shape = SubResource("RectangleShape2D_vxif3")

[node name="CollisionShape2D3" type="CollisionShape2D" parent="."]
position = Vector2(1313, 324)
shape = SubResource("RectangleShape2D_d8elw")
=======
[node name="CharacterBody2D" parent="." instance=ExtResource("1_oivte")]
position = Vector2(51, 23)
script = ExtResource("2_8siem")
>>>>>>> 59bcdf7f0c64c21a069dd8f0bef7a43b4a085b7b
```
This was special because it wasn't apart of any script so I can't just resolve the conflict like I did for the movementspeed.

Therefore, we commited this error into github and edited there to resolve the conflict and clear up the extraneous code such as `<<<<< HEAD`

## 1/10/25 [Instantiate objects](https://www.youtube.com/watch?v=Qs8oSGmhx-U)

Started off learning that instanstiating objects is a method that lets you create instances of the object:
```GDscript
func _ready():
	inst(Vector2(0,0))
	inst(Vector2(100,0))

func inst(pos):
	var instance = mynode.instantiate()
	instance.position = pos
	add_child(instance)
```
The code shows two objects that are 100 x-units away from each other

This is what I tried before the video left off how they linked the mouse to the left click to get the object to pop up everytime the mouse is clicked:
```GDscript
func _physics_process(delta):
	if Input.is_action_just_pressed("mb"):
	inst(get_global_mouse_position())

func inst(pos):
	var instance = mynode.instantiate()
	instance.position = pos
	add_child(instance)
```

Therefore, I watched another [tutorial](https://www.youtube.com/watch?v=05OixHPbxNA&t=68s) for obtaining a mouse input. I fixed the settings through project(on the top left corner)> Project Setting > Input Map

Here is the finalized version(I named the mouse "Left-Click"):
```GDscript
func _physics_process(delta):
	if Input.is_action_just_pressed("Left-Click"):
		inst(get_global_mouse_position())

func inst(pos):
	var instance = mynode.instantiate()
	instance.position = pos
	add_child(instance)
```

## 2/28/25 
I have made some progress with my project in creating a menu for placing down the buildings
Here is a break down of one of the scripts for the menu(it is used for toggling the menu)

```GDscript
extends Panel

func _input(event):
	if event.is_action_pressed("toggle_menu"):
		visible = !visible
```
To break it down, I set the "toggle_menu" as M in the Input Map, which is for events. The `!visisble` reverses the previous effects when the event happens so that the menu will pop up and collapse.

## 3/7/25
Throughout this week, I learned about the popup node and thought it'd be cool to add and make it our menu for buildings

Here are some popup methods I used:
* `popup_menu.hide()` hides the popup node named "popup_menu" which I had for showing it when a condition(user presses 'm' is met)
* `popup_menu.show(` shows the node when the user presses 'm' in the following script attached to the world scene
```
extends Node2D

var popup_menu_scene = preload("res://menu/popupmenu.tscn") 
var popup_menu : Control 
var player : Node2D 
var camera : Camera2D  

func _ready():
	popup_menu = popup_menu_scene.instantiate()
	add_child(popup_menu)
	popup_menu.hide() 
	player = get_node("CharacterBody2D")
	camera = get_node("Camera2D")

func _input(event):
	if Input.is_action_just_pressed("m"):
		toggle_menu()

func toggle_menu():
	if popup_menu.visible:
		popup_menu.hide()  # Hide the menu
	else:
		popup_menu.show()  # Show the menu
```
* The first variable, "popup_menu" is defined by taking in the scene dedicated to being a menu
* The add_child() method dynamically adds the scene for the popup menu to the world so that the player can view the menu for buildings.

## 3/7/25
For this week, I tried to polish up the functionality of the menu from last week so that I can get off the training wheels of the "testbuild.gd" file. In order to get rid of the testbuild file, I had to make the menu effective by dynamically adding the buildings without the player randomly clicking while neglecting the menu to place down the Godot icon.

```GDscricpt
func place_building(pos):
	if selected_building_scene:
		print("Placing building at:", pos)
		var instance = selected_building_scene.instantiate()
		instance.position = pos
		instance.scale = Vector2(0.33, 0.33)
		print(get_parent().get_parent().get_parent())
		get_parent().get_parent().get_parent().add_child(instance)  # Add the building to the parent scene
		can_place_building = false  # Disable building placement after placing the building
		print(can_place_building)
	else:
		print("No building selected!")
```

Yu and I made this function in a script attached to the popupmenu so that it instantiates the selected building(its scene that contains the building) which appears on the map. To put the scene on the map, we must find the map's place in the hierarchy with relativity to the script's attached file. We played around with adding as many `get_parent()` to find out when we will get to the map. First, it printed out a node2d that's the child of the sprite which makes the building move with the player which is not what we wanted. The second `get_parent()` gets the sprite which makes it a sibling, also following the user around. The next `get_parent()` gets the parent of the sprite, which is the map thus the building will stay with the map and not move with the player.

## 3/30/25
We implemented a feature that changes the cameras angle to cover more area for view when the player wants to place buildings.

```GDscript
func switch_to_map_camera():
	if map_camera:
		map_camera.make_current()
		print("Switched to MapCamera")

func switch_to_player_camera():
	if map_camera:
		player_camera.make_current()
		print("Switched to PlayerCamera")
```
The statements, when called, are helper functions that allow us to switch between the different angles. The `.make_current()` is a Godot native method that allows us to make the "current" screen. The print statements are meant for us to test when the cam angles are switched.

<!--
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
* Use ```GDscript``` for multi-line code snippets
* For images- ![menu and script](../img/whatever.png)
-->
