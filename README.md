## Godot Lesson 1
## Introduction to the animation features
- AnimationPlayer Node: The core tool used to keyframe transforms (position, rotation, scale), colors, shader parameters, visibility, audio volume, and even call methods or trigger code on a timeline. 
- AnimatedSprite2D Node: Ideal for 2D frame-by-frame or sprite-sheet animations where you switch or play sequential images.

## Code
```
extends Area2D

@export var bob_height : float = 5.0
@export var bob_speed : float = 5.0

@onready var start_y : float = global_position.y
var time : float = 0.0

func _process(delta: float) -> void:
	time += delta
	
	# Smoothly bob up and down using a sine wave
	var bob_offset = sin(time * bob_speed) * bob_height
	global_position.y = start_y + bob_offset
	
	# Simulate a 3D spin by scaling the X axis over time
	scale.x = abs(cos(time * bob_speed))

# Connect this from the Area2D Node signals panel (body_entered)
func _on_body_entered(body: Node2D) -> void:
	if body.is_in_group("Player") or body.name == "Player":
		# Optional: Add to player's inventory manager here
		# GameManager.add_coin() 
		
		# Animate collection by fading/scaling up using a Tween
		var tween = create_tween().set_parallel(true)
		tween.tween_property(self, "position:y", position.y - 30, 0.3)
		tween.tween_property(self, "modulate:a", 0.0, 0.3)
		
		# Delete the coin node once the animation completes
		tween.chain().tween_callback(queue_free)
```
