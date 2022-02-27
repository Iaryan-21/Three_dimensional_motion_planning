## Project: 3D Motion Planning
![Quad Image](./misc/enroute.png)

---
## Prerequisites

For running the project, one needs:-

Miniconda with Python 3.6. (dependning on the OS one can download and install)

## Project Description



motion_planning_from_seed_project.py: This is the based implementation for this project provided by Udacity on its seed project.
planning_utils_from_seed_project.py: It was also provided by Udacity on the its seed project. It contains an implementation of the A* search algorithm in addition to utility functions.
motion_planning.py: This version extends the provided implementation with the following features:
The global home location is read from the colliders.csv file.
The goal location is set from command line arguments (--goal_lat, --goal_lon, --goal_alt).
The calculated path is pruned with a collinearity function to eliminate unnecessary waypoints.
planning_utils.py: This file is used by motion_planning.py instead of the seed version. It provides support for the features mention above and also extends the A* search algorithm to include diagonals actions.


## Project Rubric

Explain the Starter Code

Both version are similar in the sense they implement a finite-state machine to command the drone. They are similar but not exactly the same. On the backyard_flyer_solution.py the states and transitions represented are:

![image](https://user-images.githubusercontent.com/86413005/155874363-3691e45c-5a70-41dc-933e-a89fc2c78745.png)


The state machine implemented on motion_planning.py, adds another state to the previous one:

![image](https://user-images.githubusercontent.com/86413005/155874375-225d4569-45f7-43b1-a25c-9b20205d1473.png)

## Implementation of the code:

here is a new state, PLANNING, between ARMING and TAKEOFF. When the drone is at the state ARMING and it is actually armed (line 66) on the state_callback method (lines 61 to 72), the transition to PLANNING is executed on the method plan_path. This method responsibility is to calculate the waypoints necessary for the drone to arrive at its destination.

On the plan_path method:

The map is loaded 
The grid is calculated  using the method create_grid from the module planning_utils.py.
The goal grid is set 10 north and east from local position 
To find the path to the goal, A* search algorithm is executed 1 using the a_star method from the module planning_utils.py.
The waypoints are generated  and sent to the simulator using the method send_waypoints .

## Implementation of Planning Algorithm


### In the starter code, we assume that the home position is where the drone first initializes, but in reality, you need to be able to start planning from anywhere. Modify your code to read the global home location from the first line of the colliders.csv. file and set that position as global home (self.set_home_position())

The home position is read at motion_planning.py. It use the function read_home added to planning_utils.py.

### In the starter code, we assume the drone takes off from map center, but you'll need to be able to takeoff from anywhere. Retrieve your current position in geodetic coordinates from self._latitude(), self._longitude() and self._altitude(). Then use the utility function global_to_local() to convert to local position (using self.global_home() as well, which you just set)

Post that coordinate transformation is done.

### In the starter code, the start point for planning is hardcoded as map center. Change this to be your current local position.

Grid point is then calculated.

### In the starter code, the goal position is hardcoded as some location 10 m north and 10 m east of map center. Modify this to be set as some arbitrary position on the grid given any geodetic coordinates (latitude, longitude)

Addition of parameters in motion_planning.py were done to be accept the goals coordinates. The coordinates are converted to local coordinates which are used in search agorithm

### Write your search algorithm. Minimum requirement here is to add diagonal motions to the A* implementation provided, and assign them a cost of sqrt(2). However, you're encouraged to get creative and try other methods from the lessons and beyond!

The diagonals movements were implemented by adding them to the Action enum. The valid_actions method was modified to take those actions into account.

### Cull waypoints from the path you determine using search.
The path was pruned using collinearity with the methods being explained in the course




