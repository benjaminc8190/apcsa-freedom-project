# Entry 4
##### 3/11/25

## **Context**:
We have been trying to keep on track with our plans, although some modifications had to be made. Our goal for a menu of items was somewhat completed since we have it created and has appeared on some occasions. In our next weeks, I will continue to work on it to make sure that it will appear properly when the user interacts with it. Also, my partners have been working hard to implement a tile map for our world creation and the progress seems to be looking good. For the menu, I used the [Pop-up menu node](https://www.youtube.com/watch?v=ESxRKqdu34M) from Godot and I watch about [Tilemaps for Godot](https://www.youtube.com/watch?v=ZutpG0_CYrQ&t=274s) to help with the world creation.

## Progress in my project:

### 2/28/25
Throughout this week, I have created a menu for placing down the buildings Here is a break down of one of the scripts for the menu(it is used for toggling the menu)
```GDscript
extends Panel

func _input(event):
	if event.is_action_pressed("toggle_menu"):
		visible = !visible
```

To break it down, I set the "toggle_menu" as M in the Input Map, which is for events. The !visisble reverses the previous effects when the event happens so that the menu will pop up and collapse.


### 3/7/25
Throughout this week, I learned about the popup node and thought it'd be cool to add and make it our menu for buildings

Here are some popup methods I used:
* `popup_menu.hide()` hides the popup node named "popup_menu" which I had for showing it when a condition(user presses 'm' is met)
* `popup_menu.show(` shows the node when the user presses 'm' in the following script attached to the world scene

```GDscript
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

## **Sources**:

* [Pop-up menu node](https://www.youtube.com/watch?v=ESxRKqdu34M)
* [Tilemaps for Godot](https://www.youtube.com/watch?v=ZutpG0_CYrQ&t=274s)

## **EDP**:
We are currently developing the protoype, our project, which is the fifth step of the Engineering Design Process. I have completed some parts of the plan for my project such as creating a menu for the user to see the buildings. In the upcoming weeks, I will continue to develop the game for placing the buildings down since I am currently waiting on the building designs and tilemap for the holistic world for the game. When finished, our game will be tested by our peers to ensure that there are no bugs that we overlooked which is the sixth step of the EDP.

## **Skills**:
I have continued to develop **how to learn** and **organization** throughout the fifth step of the engineering design process. I learned the concept of knowledge and application since now that I'm am doing the project, I must put my knowledge into practice and identify any knowledge gaps to continue developing the project. For example, I did not know that popup menus existed in Godot nor did I understood how it worked. That's why a quick video on the [Pop-up menu node](https://www.youtube.com/watch?v=ESxRKqdu34M) helped me understand that they can be a scene that appears as a function when the user does an action. While reviewing concepts that I have learned such as instantiating, I have found the importance in organzing my notes. This is due to the fact that I was able to relearn instantiating faster than when I first learned it. As a result, I was able to make the popup menu by using the `.instantiate()` method for replicating the scene onto the map for the user to see.

[Previous](entry03.md) | [Next](entry05.md)

[Home](../README.md)
