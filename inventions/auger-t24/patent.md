# Dynamic Dispatch Scheduler for Single-Lane Tunnel Shuttles

## Abstract
A software-based dynamic dispatch scheduler for single-lane underground shuttle loops uses real-time passenger counts from station sensors and vehicle telemetry to adjust headways and vehicle assignments. The scheduler maintains bidirectional flow with timed passing at widened sidings, targeting 4400 passengers per hour by reducing average wait to under 90 seconds.

## Problem
Single-lane tunnel loops with human-driven Tesla shuttles at 35 mph exhibit headway variability from manual dispatch, leading to bunching and underutilization. Peak recorded throughput reaches only 1355 passengers per hour against a 4400-passenger test capacity, with reported waits of 10-15 minutes during events.

## Prior art
- CN114162186A, Passenger and cargo mixed editing control method for train: describes passenger flow acquisition for mixed trains but applies to rail with fixed schedules, differing by lacking single-lane bidirectional vehicle coordination.
- US20130125778A1, Automated vehicle conveyance apparatus transportation system: covers autonomous pod conveyance but uses off-line storage rather than in-tunnel dynamic headway adjustment.

## Summary of the invention
The invention comprises a central dispatch controller (20) that receives passenger queue data from infrared counters (12) at each station platform and vehicle position/speed from onboard GPS and CAN bus. It computes minimum safe headway using tunnel length segments and issues velocity setpoints to drivers via tablet interface or future autonomy firmware. Sidings at 400 m intervals allow passing with 5-second dwell coordination.

## Claims
1. A method for operating a single-lane underground shuttle system comprising: receiving passenger counts from station sensors every 15 seconds; computing required headway as (tunnel segment length / target speed) + safety margin of 8 seconds; assigning vehicles to minimize maximum wait below 90 seconds; and transmitting velocity commands to maintain computed headway.
2. The method of claim 1 wherein safety margin is increased to 12 seconds upon detection of any vehicle speed variance exceeding 2 m/s.
3. The method of claim 1 further comprising activating siding coordination signals when two vehicles approach within 300 m.
4. The method of claim 1 wherein passenger count updates trigger reassignment if queue exceeds 18 persons.
5. The method of claim 1 implemented as firmware update to existing vehicle tablets without hardware modification.
6. The method of claim 1 logging all headway deviations for post-event analysis and threshold adjustment.

## Brief description of the drawings
FIG. 1 shows tunnel layout with stations, sidings and controller links.  
FIG. 2 shows dispatch controller data flow and vehicle interface.

## Detailed description
The system deploys infrared passenger counters (12) mounted 1.2 m above each platform edge, sampling every 15 seconds with ±2 person accuracy. Data feeds the dispatch controller (20) housed in a rack at the Westgate control room. Controller (20) segments the 3.5 km loop into 200 m blocks and calculates instantaneous headway requirement using the formula: headway = (block length / 15.6 m/s) + 8 s margin. Vehicle telemetry arrives via 4G from each Tesla Model Y CAN bus reporting speed, position and door state. When computed headway falls below 45 s the controller (20) issues a tablet alert to the nearest idle vehicle directing it to the station with queue exceeding 18 persons. Sidings widened to 4.5 m at 400 m spacing contain traffic lights (28) synchronized by controller (20) for 5 s dwell windows. Failure mode of sensor dropout defaults to fixed 90 s headway. Speed variance over 2 m/s extends margin to 12 s. All parameters are adjustable via configuration file without recompilation. The firmware runs on existing vehicle tablets using the same 12 V power already present. Every numeral referenced appears in the figures.