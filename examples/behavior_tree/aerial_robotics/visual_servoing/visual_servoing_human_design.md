# Visual Servoing using LLM-generated Behavior Tree

## The prompts 
* Environment Descriptions (Provided by Experienced Engineers)
* Robot Sensing Capability Descriptions (Provided by Experienced Engineers)
* Task Description (Provided by novice user)

```
Imagine you are a drone that can move along the YZ axes. and you’re positioned in the center of a basketball court. A person on the side of the court is going to throw a basketball ball in the air somewhere in the court, and your objective is to be at the exact XY location of the ball when it lands.

The drone has a monocular RGB camera that looks up. And it can return an image from the robot’s camera looking up. According to this image, it can also return the object's relative position inside the view of camera.  

The following subtrees are available. You are not to use any other hypothetical functions.And not all subtrees are suitable and necessary, you need to evaluate and choose the subtrees you need.

Subtrees:
"object_in_view": This behavior tree will return true when there is desired object in sight. 
"object_not_in_view": This behavior tree will return true when there is no object in sight.
"reset_drone": When this subtree is triggered, reset the preflight state that need to reset before takeoff.
"waypoint_takeoff": When this subtree is triggered, takeoff to the air.
"waypoint_landing": When this subtree is triggered, land to the ground immediately.
"waypoint_tracking": When this subtree is triggered, move toward the target object position when the target object is inside the camera view.
"linear_x_control": When this subtree is triggered, try to minimize the absolute(drone_x - object_x) when the target object is inside the camera view.
"linear_y_control": When this subtree is triggered, try to minimize the absolute(drone_y - object_y) when the target object is inside the camera view.
"linear_z_control": When this subtree is triggered, try to minimize the absolute(drone_z - object_z) when the target object is inside the camera view.
"report_error": When this subtree is triggered, report current distance (distance = (drone_x - object_X)^2 + (drone_y - object_y)^2 + (drone_z - object_z)^2)^0.5) , and the supervisor will evaulate the performance according to this distance.
"explore_forward": When this subtree is triggered, move forward a static distance.
"explore_right": When this subtree is triggered, Rotate 90 degree to right side and raise the flag. If the flag is not been reset, this subtree will not been triggered.
"explore_left": When this subtree is triggered, Rotate 90 degree to left side and raise the flag. If the flag is not been reset, this subtree will not been triggered.
"explore_rotate": When this subtree is triggered, rotate to left side and right side one time and raise the flag. If the flag is not been reset, this subtree will not been triggered.
"explore_patter_block": When this subtree is triggered, follow the pattern block pattern to explore a huge area.


Now you already in the air, don't need to takeoff. And the ball is aleady in your view, don't need any exloration behaviors. Please executes a visual servoing approach towards the ball.

Please design this task as behavior tree. You can follow the example source tasks. 
```

## Behavior Tree Designed by Human:

```
->
	(object_in_view)
	||
		?
			(linear_x_in_margin)
			[linear_x_control]
		?
			(linear_y_in_margin)
			[linear_y_control]
		?
			(linear_z_in_margin)
			[linear_z_control]
```
## Visualize Behavior Tree in graphical user interface (GUI):
![](/examples/figs/human/visual_servoing.png)

## Video: