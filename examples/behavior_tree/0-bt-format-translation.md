# Behavior Tree Format Translation

We use GPT3/ChatGPT to translate 

## Psuedo-Code Format to BT

```
I have a pseudo code and need to translate it to the script used by a behavior tree program like this:
The pseudo code:
Procedure:
    Do phase 1:{
        Do phase 1-1:{
            "object_in_view"
        }Do phase 1-2 if 1-1 success:{
            Do phase 1-2-1 at the same time{
                "linear_x_control"
                "linear_y_control"
                "linear_z_control"
            }
        }
    }Do phase 2 if 1 failed:{
        Do phase 2-1:{
            "reset_drone"
        }Do phase 2-2 if 2-1 success:{
            "waypoint_takeoff"
        }Do phase 2-3 if 2-2 success:{
            "explore_pattern_block" 
        } 
    }
Script:
?
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
    ->
        ?
            (state_offboard)
            ->
                ?
                    <!>
                        (waypoint_takeoff_finished)
                    [waypoint_takeoff_reset]
        ->
            (state_offboard)
            ?
                (waypoint_takeoff_finished)
                [waypoint_takeoff]
        ->
            ?
                (explore_pattern_block_finished)
            [explore_pattern_block]
Could you help me to translate another pseudo code?
```

Now you could input your pseudo-code format 

```
Procedure:
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
```
ChatGPT
Sure, here's the translation of the provided pseudo code into a behavior tree script:

```
->
    ?
        (already_visited_destination)
    ->
        ?
            (already_at_home)
        [already_at_home_action]
    ->
        ?
            (land)
        [land_action]
->
    !
        ?
            (already_visited_destination)
        ->
            ?
                (already_in_the_air)
            ->
                ?
                    (takeoff)
                [takeoff_action]
        ->
            ?
                (already_visited_destination)
            ->
                ?
                    (go_to_destination)
                [go_to_destination_action]
        ->
            ?
                (already_at_home)
            ->
                ?
                    (go_home)
                [go_home_action]

```




