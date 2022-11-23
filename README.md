# ROS Path Planning

## Summary

### What the task is about

In this Path Planning Project, we are going to use `ROS` to design a path-planninging feedback-control navigation program for the `Turtlebot3` to go to the destination automatically while not fully knowing the environment. In addition, after the `Turtlebot3` can successfully go to the destination, we are also required to enhance the program in order to reduce the time taken and number of unsuccessful trials.

### Enhancement I have done

I enhanced the path-planning program by **adjusting the order of checking neighbors** in `std::vector<std::array<int, 2>> findPath(int idx_x, int idx_y, int goal_x, int goal_y)` in `path_plan_node.cpp`. The program after enhancement will check west neighbor first before checking north neighbor, and this is to prevent the `Turtlebot3` from going to the wrong north direction and having to make a U turn back at cell `(5, 1)`.

## Initial Conditions

![Screenshot%20from%202022-11-01%2014-54-34.png](https://github.com/Yu-Haikuo/Robotics-ROS-Path-Planning/blob/main/Figures/Screenshot%20from%202022-11-01%2014-54-34.png)

The assigned destination according to my matriculation number is `(3, 4)`. The `goal_x` is `3.5` and `goal_y` is `4.5`.

## Design of Path Planning

The path-planning program uses `BFS` algorithm to find all available paths on a breadth-first basis with the help of a priority queue. The algorithm will initialize all nodes first, then put the nodes into the queue. After that the queue will keep dequeuing until the goal is found or the queue is empty.

In this process, it will access the first element in the queue, and then delete the first element from the queue. Then if the goal is found, the loop will end. If not, then search the neighbours and queue them if the cost is fewer.

## ROS Structure and Implementation
![Screenshot%20from%202022-11-01%2016-50-52.png](https://github.com/Yu-Haikuo/Robotics-ROS-Path-Planning/blob/main/Figures/Screenshot%20from%202022-11-01%2016-50-52.png)

Three main nodes: 
* `path_plan_node`
* `bot_control_node`
* `range_detect_node`

## Performance

<p align="center">
  <img height="450" width="800" src="https://github.com/Yu-Haikuo/Robotics-ROS-Path-Planning/blob/main/Figures/ezgif.com-gif-maker.gif">
</p>

The `best result` is defined as the least time needed for the `Turtlebot3` to go to the correct destination. Therefore, for this best result of the simulation (after enhancement), the total time taken is around `54.556s`.

## Navigation and Tuning

I applied **Proportional Control (P Control)** first for both position error and heading error. Then after tuning I found that Proportional does not have a good effect for heading error, so I applied **Proportional-Integral Control (PI Control)**, and a good result was obtained. Hence, `Kp_x` is set to `0.5` while `Ki_x` and `Kd_x` are set to `0`, and `Kp_a` is set to `1.5` and `Ki_a` is set to `1` while `Kd_a` is set to `0`.

## Enhancement

### What I had planned to improve

![Without%20Enhancement.jpg](https://github.com/Yu-Haikuo/Robotics-ROS-Path-Planning/blob/main/Figures/Without%20Enhancement.jpg)

Before the enhancement, the `Turtlebot3` will go directly to the north direction since the start and then have a U turn back at cell `(5, 1)` shown in the figure above. This will waste more time and delay the whole path-planning process. Hence, I planned to adjust the path-planning algorithm to avoid this wrong trial to save some time.

### How I did it

I adjusted the order of checking neighbors by modifying the code in `std::vector<std::array<int, 2>> findPath(int idx_x, int idx_y, int goal_x, int goal_y)` in `path_plan_node.cpp`. The original direction checking order is `north -> west -> south -> east`, which means the program will check if north wall does not exist and north cell is accessible, then control the `Turtlebot3` to go north; if north direction is not approachable, then it will check the west direction, and so are the rest.

I changed the direction checking order to `west -> north -> south -> east` by changing the order of relative code block inside `while(!queue.empty()){}` to achieve the enhancement.

### Result

Before the enhancement, the `Turtlebot3` will go directly to the north direction since the start, and then make a U turn at cell `(5, 1)` as shown in the figure above, and the total time taken is `1min 07s`.

After the enhancement, the `Turtlebot3` will not go to wrong direction and a U turn is no longer needed. Instead, it will proceed to the correct west direction directly as shown in the figure above, and the total time taken is reduced by to `54s`, which is improved by `19.4%` compared to previous one without the enhancement.
