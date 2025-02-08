# Entry 3
##### 02/03/25

## **Context**:
Before the Winter Break, my partners and I brainstormed many plans for progress on our project and learning more about our tool. After so many changes to our original ideas and plans to find a promising solution that's feasible for us to do within just a few days before vacation. Although we weren't able to complete all our goals, we managed some progress for our project. Since the [FreeCodeCamp tutorial](https://www.youtube.com/watch?v=S8lMTwSRoRg&t=2496s) was too long, I skipped ahead and saw that that was not a game I was trying to make so I took away some key concepts instead just as animations, importing assets, and the title screen where I practiced using buttons then moved on to other short videos for features that we want in our projects. I learned about how to [instantiate objects](https://www.youtube.com/watch?v=Qs8oSGmhx-U&t=2s) to get the concept of allowing the player to build buildings and learning to use [Github Desktop](https://www.youtube.com/watch?v=fZ-CJIYPFMI&t=57s) for sharing and change controls with my partners.

## **What I have been doing in the past few weeks since 12/11/24**:

## [Merge Conflict testing](https://www.youtube.com/watch?v=fZ-CJIYPFMI&t=215s)
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

## [Instantiate objects](https://www.youtube.com/watch?v=Qs8oSGmhx-U)

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

## **Plan**:
* Make a map for the world
* Make a menu for constructing buildings



## **Sources**:

* [Getting input](https://www.youtube.com/watch?v=05OixHPbxNA&t=68s)
* [instantiate objects](https://www.youtube.com/watch?v=Qs8oSGmhx-U&t=2s)
* [Github Desktop](https://www.youtube.com/watch?v=fZ-CJIYPFMI&t=57s)

## **EDP**:

I am on the fourth step of the engineering design process(EDP) as I plan the most promising solution for my game as I narrow down on what features I would like to add for the game. So far we have already implemented some features such as instantiating for buidling buildings. We are still planning out what features we want in our game to fully resonate with our ideals for historical focus. Our idea of MVP has been very shaky due to many changes in the plans to adjust for our tight work schedule. I cannot plan to do much for the upcoming midwinter since I will be going to vacation and I will have a lot of work to catch up on from the senior trip so our plans will most likely change again. However, we are coming close to the fifth step of the engineering design process where we create the prototype, which is our game. 

## **Skills**:

Some skills that I have picked up during the fourth step of the EDP were **how to learn** and **organization** from managing the plans of the project and finding youtube videos to learn specific features I want to add. While doing this project, I am learning more about myself and how I take in information. I felt that I learned more from listening to youtube videos in my free time rather than reading the documentaries and articles from online. That's why I watched a lot of videos for this project such as [how to get input for Godot](https://www.youtube.com/watch?v=05OixHPbxNA&t=68s), [instantiating objects](https://www.youtube.com/watch?v=Qs8oSGmhx-U&t=2s), and [how to use Github Desktop](https://www.youtube.com/watch?v=fZ-CJIYPFMI&t=57s). Also, I have been trying to organize what we need/want in the project for a feasible MVP that we can commit. As this is our senior year, there are many acitivies that I want to participate in and have a lot to do due to multiple AP classes. Therefore, I have been working hard to organize our ideas into categories of how much we need or just simply want for fun.

[Previous](entry02.md) | [Next](entry04.md)

[Home](../README.md)
