# Inspection using LLM-generated Behavior Tree

## The prompts 
* Environment Descriptions (Provided by Experienced Engineers)
* Robot Sensing Capability Descriptions (Provided by Experienced Engineers)
* Sample Template (Provided by Experienced Engineers)
* Task Description (Provided by novice user)

```
Imagine you are helping me interact with the Gazebo simulator for drones. At any given point of time, you have the following abilities. 
There are two turbines, some solar panels and a car in the world.

The simulator contains a drone, along with several objects. Apart from the drone, none of the objects are movable. 

The following subtrees are available. You are not to use any other hypothetical functions.And not all subtrees are suitable and necessary, you need to evaluate and choose the subtrees you need.

Subtrees:
"object_in_view": This behavior tree will return true when there is desired object in sight. 
"object_not_in_view": This behavior tree will return true when there is no object in sight.
"reset_drone": When this subtree is triggered, reset the preflight state that need to reset before takeoff.
"set_altitude": When this subtree is triggered, set the altitude of cruise drone as 5 meters.
"waypoint_takeoff": When this subtree is triggered, takeoff to the air as the desired height.
"waypoint_landing": When this subtree is triggered, land to the ground immediately.
"waypoint_tracking": When this subtree is triggered, move toward the target object position when the target object is inside the camera view.
"linear_x_control": When this subtree is triggered, try to minimize the absolute(drone_x - object_x) when the target object is inside the camera view.
"linear_y_control": When this subtree is triggered, try to minimize the absolute(drone_y - object_y) when the target object is inside the camera view.
"linear_z_control": When this subtree is triggered, try to minimize the absolute(drone_z - object_z) when the target object is inside the camera view.
"report_error": When this subtree is triggered, report current distance (distance = (drone_x - object_X)^2 + (drone_y - object_y)^2 + (drone_z - object_z)^2)^0.5) , and the supervisor will evaulate the performance according to this distance.
"explore_forward": When this subtree is triggered, move forward a static distance.
"rotate_right": When this subtree is triggered, Rotate 90 degree to right side and raise the flag. If the flag is not been reset, this subtree will not been triggered.
"rotate_left": When this subtree is triggered, Rotate 90 degree to left side and raise the flag. If the flag is not been reset, this subtree will not been triggered.
"explore_rotate": When this subtree is triggered, rotate to left side and right side one time and raise the flag. If the flag is not been reset, this subtree will not been triggered.
"explore_pattern_block": When this subtree is triggered, follow the pattern block pattern to explore a huge area (equal to the size of solar panel).



And here are the example source tasks in behavior tree architecture:


Task: change wheel for a car
Procedure:{
    Do phase 1:{
        Do phase 1-1:{
            "put_car_at_conveyor"
        }Do phase 1-2 if 1-1 success:{
            "lift_car"
        }
    }Do phase 2 if 1 success:{
        Do phase 2-1:{
            "pick_wheel"
        }Do phase 2-2 if 2-1 success:{
            "approach_conveyor"
        }
    }Do phase 3 if 2 success:{
        Do phase 3-1 at the same time:{
            "align_wheel_in_x_axis"
            "align_wheel_in_y_axis"
        }
    }Do phase 4 if 3 success:{
        Do phase 4-1:{
            "insert_screws"
        }Do phase 4-2 if 4-1 success:{
            "fasten_screws"
        }
    }Do phase 5 if 4 success:{
        Do phase 5-1:{
            "leave_conveyor"
        }
    }
}

Task: Take off and fly to the desired destination. After reach the destination, go home.
Procedure:{
    Do phase 1:{
        Do phase 1-1:{
            "already_visited_destination"
        }Do phase 1-2 if 1-1 success:{
            "already_at_home"
        }Do phase 1-3 if 1-2 success:{
            "land"
        }
    }Do phase 2 if 1 failed:{
        Do phase 2-1:{
            Do phase 2-1-1:{
                "already_in_the_air"
            }Do phase 2-1-2 if 2-1-1 success:{
                "takeoff"
            }
        }Do phase 2-2 if 2-1 success:{
            Do phase 2-2-1:{
                "already_visited_destination"
            }Do phase 2-2-2 if 2-2-1 success:{
                "go_to_destination"
            }
        }Do phase 2-3 if 2-2 success:{
            Do phase 2-3-1:{
                "already_at_home"
            }Do phase 2-3-2 if 2-3-1 success:{
                "go_home"
            }
        }
    } 
}

Now you already in the air, don't need to takeoff. This solar panel array is 30 meters wide and 50 meters long. 
I want you to fly the drone in a lawnmower pattern above this array of solar panels to inspect them at an altitude of 5 meters.

Please design this task as behavior tree. You can follow the example source tasks. 
```

## Behavior Tree Generation:

Presented below is a behavior tree generated by the Large Language Model (LLM) in a "pseudo-code" format.

The result is generated by ChatGPT (GPT-3.5) on different occasions, offering a diverse set of behavior tree outputs.

```
Procedure: {
    Do phase 1: {
        Do phase 1-1: {
            "object_not_in_view"
        } Do phase 1-2 if 1-1 success: {
            "set_altitude"
            "waypoint_tracking"
        }
    } Do phase 2 if 1 success: {
        Do phase 2-1: {
            Do phase 2-1-1: {
                "object_in_view"
            } Do phase 2-1-2 if 2-1-1 success: {
                "linear_x_control"
            }
        } Do phase 2-2 if 2-1 success: {
            "explore_forward"
        } Do phase 2-3 if 2-2 success: {
            "rotate_right"
        }
    } Do phase 3 if 2 success: {
        Do phase 3-1: {
            Do phase 3-1-1: {
                "object_in_view"
            } Do phase 3-1-2 if 3-1-1 success: {
                "linear_y_control"
            }
        } Do phase 3-2 if 3-1 success: {
            "explore_forward"
        } Do phase 3-3 if 3-2 success: {
            "rotate_left"
        }
    } Do phase 4 if 3 success: {
        Do phase 4-1: {
            Do phase 4-1-1: {
                "object_in_view"
            } Do phase 4-1-2 if 4-1-1 success: {
                "linear_x_control"
            }
        } Do phase 4-2 if 4-1 success: {
            "explore_forward"
        } Do phase 4-3 if 4-2 success: {
            "rotate_right"
        }
    } Do phase 5 if 4 success: {
        Do phase 5-1: {
            Do phase 5-1-1: {
                "object_in_view"
            } Do phase 5-1-2 if 5-1-1 success: {
                "linear_y_control"
            }
        } Do phase 5-2 if 5-1 success: {
            "explore_forward"
        } Do phase 5-3 if 5-2 success: {
            "rotate_left"
        }
    } Do phase 6 if 5 success: {
        Do phase 6-1: {
            "waypoint_landing"
        }
    }
}
```

## The Translated Behavior Tree for Direct Execution in Simulator:

We employ a parser to translate the answer generated by the LLM into a script that can be readily executed by a ROS node.

*  By using the function: "post_processing" and "subtree_assembly"in [behavior_tree_generation/src/utils.py](https://github.com/ARG-NCTU/behavior_tree_generation/blob/master/src/utils.py),  it can translate the origin output to the format that can be executed directly.
```
from utils import post_processing, subtree_assembly
...
output_file = subtree_assembly(post_processing(string_generated_by_LLM), path_sub_tree_dir)
...
```

```
->
	->
		->
			<!>
				(object_in_view)
		?
			(set_altitude_finished)
			[set_altitude]
	->
		->
			->
				(object_in_view)
			?
				(linear_x_in_margin)
				[linear_x_control]
		?
			(explore_forward_finished)
			[explore_forward]
		?
			(rotate_right_finished)
			[rotate_right]
	->
		->
			->
				(object_in_view)
			?
				(linear_y_in_margin)
				[linear_y_control]
		?
			(explore_forward_finished)
			[explore_forward]
		?
			(rotate_left_finished)
			[rotate_left]
	->
		->
			->
				(object_in_view)
			?
				(linear_x_in_margin)
				[linear_x_control]
		?
			(explore_forward_finished)
			[explore_forward]
		?
			(rotate_right_finished)
			[rotate_right]
	->
		->
			->
				(object_in_view)
			?
				(linear_y_in_margin)
				[linear_y_control]
		?
			(explore_forward_finished)
			[explore_forward]
		?
			(rotate_left_finished)
			[rotate_left]
	->
		?
			(waypoint_landing_finished)
			[waypoint_landing]
```
## Visualize Behavior Tree in graphical user interface (GUI):
![](/examples/figs/chatgpt/inspection_trial_2.png)

## Video:


