# Centralized Software Controller for Autonomous Tunnel Fleet Coordination

## Abstract
A centralized software controller manages Tesla Model Y fleets in single-lane underground tunnels by dynamically allocating virtual slots, computing safe headways via V2I links, and executing trajectory commands over cellular. The system replaces human drivers with firmware-based autonomy, raising throughput to 1200 vehicles per hour per lane at 35 mph while maintaining 2-second minimum separation.

## Problem
Human drivers limit Vegas Loop capacity to roughly 4400 passengers per hour because each vehicle requires manual operation at 35 mph in confined 12-foot tunnels. Station dwell times and reaction delays create bottlenecks. Software updates alone have not enabled full autonomy due to lack of coordinated fleet control.

## Prior art
- US20180188031A1, System and method for calibrating vehicle dynamics expectations for autonomous driving, discloses individual vehicle trajectory monitoring but lacks tunnel-wide slot allocation and V2I headway enforcement.
- US20230166770A1, Trajectory determination for four-wheel steering, covers local vehicle models without centralized tunnel orchestration or passenger-count-based throughput optimization.
- EP3782000B1, A method for controlling a string of vehicles, describes V2V platooning but omits single-lane underground constraints and station throughput scheduling.

## Summary of the invention
The invention is a server-based fleet controller that assigns time-space slots to vehicles, transmits trajectory setpoints at 10 Hz via 5G, and monitors lidar and wheel odometry to enforce separation. It integrates passenger load sensors to optimize dwell and reroute empty vehicles.

## Claims
1. A method for autonomous operation of electric vehicles in a single-lane tunnel comprising: a central controller receiving vehicle position data at 100 ms intervals; computing required headway of at least 2 seconds at 35 mph; transmitting acceleration and steering commands to maintain slot assignment; and adjusting commands upon detection of sensor fault within 200 ms.
2. The method of claim 1 further comprising passenger load sensors reporting occupancy to the controller every 5 seconds for dynamic slot reallocation.
3. The method of claim 1 wherein the controller maintains a virtual slot map updated at 10 Hz with position tolerance of 0.5 m.
4. The method of claim 1 further comprising fallback to 10 mph creep mode upon loss of primary communication exceeding 500 ms.
5. The method of claim 1 wherein station dwell is limited to 15 seconds by predictive arrival scheduling.

## Brief description of the drawings
FIG. 1 shows the tunnel cross-section with vehicle, slot boundaries, and communication links. FIG. 2 shows the software architecture block diagram with data flows.

## Detailed description
The central controller (20) runs on a redundant server cluster located at the tunnel operations center. Each Tesla Model Y (10) is equipped with a firmware module that receives trajectory commands over 5G from the controller. Position is determined by wheel odometry fused with tunnel-fixed beacons spaced 50 m apart, achieving 0.3 m accuracy. At 35 mph (15.65 m/s) the minimum headway distance is 31.3 m corresponding to 2 seconds. The controller (20) maintains a slot table updated every 100 ms. When a vehicle (10) reports occupancy via seat weight sensors (12), the controller reallocates downstream slots to maximize throughput. Upon communication loss exceeding 500 ms the vehicle firmware reduces speed to 10 mph creep using local lidar. Station arrival prediction uses current speed and distance to schedule 15-second dwell windows. Materials for beacon mounts are stainless steel brackets torqued to 12 Nm. Failure mode of beacon occlusion triggers fallback to odometry with 1 m tolerance for 30 seconds before safe stop. All dimensions maintain 0.5 m lateral clearance in the 3.66 m wide tunnel bore.