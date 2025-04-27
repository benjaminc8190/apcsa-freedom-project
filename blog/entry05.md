# Entry 5
##### 4/22/25

## **Context**:
During the Spring Break, my partners and I have been working diligently to have a finalized "barebone" of the project since the deadline for the Minimum Viable Product(MVP) was on 4/21/25. As we worked on the final features that we wanted in our game, we looked back into [control nodes for pop-up menus](https://www.youtube.com/watch?v=5Hog6a0EYa0&t=440s) for the characteristics of buildings from our previous main menu. Since our plans were set, we mainly had to deal with implementing the features such as wood as our resource, or "currency" of our game so far. As a "barebone" we also had to make sure there were little to no bugs and crashes which is why we left miniature details out such as the player walking through trees and random invisible blocks where the player cannot move through.

## Progress in the project:

### Lumberyard Menu:

My partners and I worked on a wood menu intending to follow an idea we saw from Frostpunk which is to be able to allocate workers to factories or workplaces in order to boost production. Therefore, we made it so that the lumberyard would be useless without worker allocation as well as a maximum worker count for each lumberyard forcing the player to place more when they have reached the capacity of workers. We made to buttons for the player to remove or add workers as well as labels to tell the player the amount of workers and the building's maximum capacity. These were all children nodes of a panel, a node that we commonly used for all menus.
```GDscript
extends Control

@onready var population_label = $VBoxContainer/MarginContainer/population
@onready var production_label = $VBoxContainer/MarginContainer2/production
@onready var assign_button = $Add
@onready var remove_button = $Remove
@onready var exit_button = $exit
@onready var interact_label = $"../../../Node2D/TileMapLayer/trunk/CharacterBody2D/Interaction Component/InteractLabel"
@onready var player = $"../../../Node2D/TileMapLayer/trunk/CharacterBody2D"
var assigned_workers := 0
var max_workers := 5
var wood_per_worker := 1

func _ready():
	hide()
	assign_button.pressed.connect(assign_worker)
	remove_button.pressed.connect(remove_worker)
	exit_button.pressed.connect(exit_pressed)

func assign_worker():
	if assigned_workers < max_workers and ResourceManager.resources["unemployed"] > 0:
		assigned_workers += 1
		ResourceManager.resources["unemployed"] -= 1
		ResourceManager.add_production("wood", wood_per_worker)
		population_label.text = "Workforce Population: "  + str(assigned_workers) + "/" + str(max_workers)
		production_label.text = "Production rate: " + str(assigned_workers)+"/" + "s"
	else:
		print("No available population or max workers reached")

func remove_worker():
	if assigned_workers > 0:
		assigned_workers -= 1
		ResourceManager.resources["unemployed"] += 1
		ResourceManager.add_production("wood", -(wood_per_worker))
		population_label.text = "Workforce Population: "  + str(assigned_workers) + "/" + str(max_workers)
		production_label.text = "Production rate: " + str(assigned_workers)+"/" + "s"
	else:
		print("No more workers")

func disable_player_movement():
	player.set_interacting_state(true)
func enable_player_movement():
	player.set_interacting_state(false)

func exit_pressed():
	interact_label.show()
	enable_player_movement()
	hide()
```
### Main Menu Rework:
Before the break, we have developed and basis of the game, where the player is able to find the buildings they want to build. We were able to test the menu by having a building, called house, established. However, this we also added resources as a form of currency, we wanted to reflect real life where it costs materials to build something. Which is why we added an if statement that checks when the player has enough wood to build a building. 
```GDscript
extends Control
class_name BuildingMenu
# Signal emitted when a building is selected
signal building_selected(building_scene)

@onready var player_camera = get_node("../trunk/CharacterBody2D/Camera2D")  # Player's camera
@onready var map_camera = get_node("../MapCamera")  # Map-view camera
@onready var player = get_node("../trunk/CharacterBody2D/")

# List of available buildings
var buildings = [
	{"name": "House", "cost": 10, "scene": preload("res://Buildings/house/house.tscn")},
	{"name": "Lumberyard", "cost": 10, "scene": preload("res://Buildings/lumberyard/LumberYard.tscn")},
]


# Variables for building placement
var can_place_building := false
var selected_building_scene : PackedScene
var building_preview : Node2D  # This will hold our preview instance
var preview_visible := false

var placed_obstacles = {}

func _ready():
	# Add buttons for each building in the Popup menu
	var container = $Control/Panel/VBoxContainer
	for building in buildings:
		var btn = Button.new()
		btn.text = building["name"] + " (" +str(building["cost"]) + " wood)"
		btn.custom_minimum_size = Vector2(200, 100)
		btn.connect("pressed", Callable(self, "_on_building_selected").bind(building["scene"]))
		container.add_child(btn)
	
	if map_camera:
		player_camera.make_current()  # Enable the player's camera
	# Hide the menu initially
	
	var tree_tile_ids = [2]  # Replace with your actual tree tile IDs
	for pos in $"../trunk".get_used_cells():  # 0 = layer index
		var tile_id = $"../trunk".get_cell_source_id(pos)
		if tile_id in tree_tile_ids:
			placed_obstacles[pos] = "trees"
		
	#print(placed_obstacles)
	hide()

	

func _process(delta):
	if can_place_building:
		update_building_preview()
	

func _on_building_selected(building_scene):
	for building in buildings:
		if building["scene"] == building_scene:
			var cost = building["cost"]
			if ResourceManager.resources["wood"] >= cost:
				selected_building_scene = building_scene
				can_place_building = true

				if is_instance_valid(building_preview):
					building_preview.queue_free()
					building_preview = null

				# Create preview instance
				building_preview = selected_building_scene.instantiate()
				building_preview.modulate = Color(1, 1, 1, 0.5)
				get_parent().get_parent().get_parent().add_child(building_preview)
				preview_visible = true
			#else:
				#print("Not enough wood to select this building.")
			break
	
func update_building_preview():
	if building_preview:
		var mouse_position = get_global_mouse_position()
		building_preview.position = mouse_position
		building_preview.scale = Vector2(0.33, 0.33)
	if ResourceManager.resources["wood"] < 10:
		cancel_building_placement()
		

func place_building(pos):
	if not selected_building_scene:
		#print("No building selected!")
		return

	# Get cost of the selected building
	var cost = 0
	for building in buildings:
		if building["scene"] == selected_building_scene:
			cost = building["cost"]
			break

	if ResourceManager.resources["wood"] < cost:
		#print("Not enough wood to place this building.")
		return

	# Proximity check
	for building in placed_obstacles:
		if building.distance_to($"../../TileMapLayer".local_to_map(
		$"../../TileMapLayer".to_local(
			$"../../TileMapLayer/MapCamera".get_screen_transform().affine_inverse() 
			* get_viewport().get_mouse_position()))) < 4 or $"../../TileMapLayer".local_to_map(
		$"../../TileMapLayer".to_local(
			$"../../TileMapLayer/MapCamera".get_screen_transform().affine_inverse() 
			* get_viewport().get_mouse_position())).y > 9:
			print("Sorry, you can't place it there at", pos)
			return
			
	
	# Place the building
	var instance = selected_building_scene.instantiate()
	instance.position = pos
	instance.scale = Vector2(0.33, 0.33)
	get_parent().get_parent().get_parent().add_child(instance)
	placed_obstacles[get_building_position()] = instance
	ResourceManager.resources["wood"] -= cost
	
	

func _physics_process(delta):
	if can_place_building and Input.is_action_just_pressed("LMC"):
		var mouse_position = get_global_mouse_position()
		place_building(mouse_position)
	
	# Right click to cancel placement
	if can_place_building and Input.is_action_just_pressed("RMC"):
		cancel_building_placement()

func cancel_building_placement():
	can_place_building = false
	if is_instance_valid(building_preview):
		building_preview.queue_free()
		building_preview = null

func disable_player_movement():
	player.set_interacting_state(true)
func enable_player_movement():
	player.set_interacting_state(false)
func switch_to_map_camera():
	if map_camera:
		map_camera.make_current()
func switch_to_player_camera():
	if map_camera:
		player_camera.make_current()

func open_menu():
	disable_player_movement()
	switch_to_map_camera()
	show()
func close_menu():
	enable_player_movement()
	switch_to_player_camera()
	cancel_building_placement()
	hide()
func _input(event: InputEvent) -> void:
	if event.is_action_pressed("open_menu"):
		open_menu()
	if event.is_action_pressed("escape"):
		close_menu()
		
func get_building_position():
	return $"../../TileMapLayer".local_to_map(
		$"../../TileMapLayer".to_local(
			$"../../TileMapLayer/MapCamera".get_screen_transform().affine_inverse() 
			* get_viewport().get_mouse_position()))
```

### Resource Manager:
In order to keep track of our resources, we made a script that has two arrays: total resource and their production rates. In order to produce constantly, we used the delta value, which is how many frames our ame is running on, and adding them up until it reaches at least one, meaning a second. This kept the rates producing and allowed us to increase the rates with how many workers we added to a lumberyard from the lumberyard menu script. To actually allow the code to work in all our files, the `add_production(resource_name: String, amount: int)` and `produce_resources()` had to be public so that our nodes and other scripts are able to call them and increase/decrease the rates. This is where Godot's autoload comes in handy. To enable autoload for all our files in the game, we simply go to project on the top left corner of the editor to access its settings than go to globals and make the script a global variable so that its functions can be called.
```GDscript
extends Node

var resources = {
	"wood": 120,
	"unemployed": 6,
	"population": 6,
}

var production_rates = {
	"wood": 0,
	"unemployed": 0,
	"population": 0,
}

var production_interval := 1.0
var time_accumulator := 0.0

# Called when the node enters the scene tree for the first time.
func _ready() -> void:
	pass # Replace with function body.


# Called every frame. 'delta' is the elapsed time since the previous frame.
func _process(delta):
	time_accumulator += delta
	if time_accumulator >= production_interval:
		produce_resources()
		time_accumulator = 0

func produce_resources():
	for res in production_rates:
		resources[res] += production_rates[res]
		#print("Produced %d %s. Total: %d" % [production_rates[res], res, resources[res]])
		
func add_production(resource_name: String, amount: int):
	if production_rates.has(resource_name):
		production_rates[resource_name] += amount
	else:
		production_rates[resource_name] = amount
```

## **Next Steps(Beyond MVP)**:
* Tree removal for player expansion
* More buildings for diversified resources: farms, mines 
* Population increase systems/features
* Shop/Merchant near the shore to trade for food
* Notification system that tells the player stuff.
* Remove buildings feature

## **Sources**:

* [Control Nodes](https://www.youtube.com/watch?v=5Hog6a0EYa0&t=440s)
* [Autoload](https://www.youtube.com/watch?v=oNjaTYoSzBY)

## **EDP**:
We have established a "final" product throughout the Spring break, which is step 5 of the engineering design process. For us, we have some peers go through our website which is the sixth step of the engineering design process, testing our product. We have noticed, from their feedback, that there is a lot of content missing that shows irrelevance to the time period we wanted. We also had to figure out how our population system will work because we were too focus on getting our "wood" resources to accumulate for the player. We will be entering the seventh step of the engineering design process where we address those concerns in order to improve our game.

## **Skills**:
While coding alongside my partners for 3 dayss straight during the break, we had to adapt to each other's lifestyle such as sleep schedule. As a result, I developed the skills **Collaboration** and **Consideration**. When creating our project, we had to think about the intent for why we created the project, an educational game, in reflection to the colonial era of America. We had to discuss with each other how we want our website to convey to the player and make agreements on what time we have left to create our brilliant ideas.

[Previous](entry04.md) | [Next](entry06.md)

[Home](../README.md)
