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

