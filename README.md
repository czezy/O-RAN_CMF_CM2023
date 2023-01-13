# O-RAN_CMF
Conflict Mitigation Framework and Conflict Detection in O-RAN nRT-RIC
ComMag 2023

## Simulation scenario
1. base stations and simulation area:
    - 19 base stations (BS) distributed on a hexagonal grid
    - distance between each BS is equal to 600 meters
    - simulation area is limited with a polygonal shapre approximating coverage distance of the network
    - coverage provided by each BS is not split into sectors
    - all base stations have the same parameters:
        - antenna gain: 2 dB
        - antenna height: 10 m
        - cable loss: 2 dB
        - TX power: 28 dBm
        - centre frequency band: 2100 MHz 
        - bandwidth: 20 MHz
        - subcarrier count: 12
        - subcarrier spacing: 15 kHz
        - default Cell Individual Offset: 0 dB
        - default handover Time-To-Trigger: 0.064 s
        - default handover hysteresis: 0 dB
 
    ![Simulation area and distributed base stations](/figures/base_stations.png)
    
2. users:
    - 380 randomly distributed across the simulation area (20 users per base station)
    - each user has a randomly assigned user profile deciding his demanded bitrate (user profiles defined below)
    - 80% of users are pedestrians, 20% of users are moving in vehicles
    - users move randomly around the simulation area with constant movement speed (5 m/s for pedestrians, 25 m/s for vehicles)
    - user position is updated each 0.05 s of simulation time
    - movement direction is changed on each position update with probability of 0.06% or when area boundary is reached with probability of 100%
    - all user equipment have the same parameters:
        - antenna gain: 0 dB
        - antenna height: 1.6 m
        - cable loss: 0 dB
        - MIMO configuration: 2x2
        - RX sensitivity: -80 dBm

    ![Considered user distribution](/figures/users.png)

3. user profiles:
    - each profile defines user's demanded bitrate, i.e., the bitrate that user requires from the network to suffice for his data throughput requirements
    - the user profiles are assigned at random, with prespecified probabilities
    - three user profiles:
        - low bitrate: 96 kbps, 60% probability (green color)
        - medium bitrate: 5000 kbps, 30% probability (yellow color)
        - high bitrate: 24000 kbps, 10% probability (red color)

4. traffic generation:
    - each user generates network traffic following a Poisson process
    - time between connection attempts:
        - mean: 20 s
        - standard deviation: 3 s
    - connection duration:
        - mean: 60 s
        - standard deviation: 15 s
    - connection is established only when any base station has enough radio resources to handle the connection (free physical resource blocks)
    - connections can be handed over between base stations once an A3 event is triggered

5. propagation model:
    - UMa (urban macro) model from the 38.901 3GPP Technical Report is utilised for pathloss calculations
    - model parameters:
        - body loss: 1 dB
        - slow-fading margin: 4 dB
        - foliage loss: 4 dB
        - interference margin: 2 dB
        - rain margin: 0 dB

6. RIC configuration:
    - all base stations are managed with a single nRT-RIC; NRT-RIC is not simulated
    - 2 xApps are deployed simultaneously: Mobility Robustness Optimizaztion (MRO) and Mobility Load Balancing (MLB)
    - MRO monitors handover statistics of each base station and modifies handover Hysteresis and Time-To-Trigger parameters to minimize the number of Radio Link Failures (RLFs) and ping-pong handovers (i.e., handovers that from BS #1 to BS #2 and then from BS #2 to BS #1 happening within a set period)
        - the ping-pong handover period in the simulation scenario is 10 seconds
        - handover TTT parameter values are chosen for specific base stations based on their ratio of ping-pong handovers to all handovers from last 240 seconds (table with details below)
        - handover hysteresis parameter values are chosen for specific base stations based on their ratio of radio link failures (RLF) to all handovers from last 240 seconds (table with details below)
    - MLB balances load of the base stations in the network, choosing Cell Individual Offset (CIO) values according to load of the base stations
        - handover Cell Individual Offset values are chosen for specific base stations based on their current load (i.e., number of currently utilized physical resource blocks to all physical resource blocks) (table with details below)

MRO - ping-pong handover ratio to TTT value mapping
| Ping-pong HO ratio range | TTT value (s) |
| --- | --- |
| 0.00% - 15.00% | 0.064 |
| 15.00% - 20.00% | 0.08 |
| 20.00% - 25.00% | 0.1 |
| 25.00% - 30.00% | 0.128 |
| 30.00% - 35.00% | 0.16 |
| 35.00% - 40.00% | 0.256 |
| 40.00% - 45.00% | 0.32 |
| 45.00% - 50.00% | 0.48 |
| 50.00% - 55.00% | 0.512 |
| 55.00% - 60.00% | 0.64 |
| 60.00% - 65.00% | 1.024 |
| 65.00% - 70.00% | 1.28 |
| 70.00% - 100.00% | 2.56 |

MRO - RLF ratio to hysteresis value mapping
| RLF ratio range | Hysteresis value (dB) |
| --- | --- |
| 0.00% - 3.75% | 0 |
| 3.75% - 7.50% | 0.5 |
| 7.50% - 11.25% | 1 |
| 11.25% - 15.00% | 1.5 |
| 15.00% - 18.75% | 2 |
| 18.75% - 22.50% | 2.5 |
| 22.50% - 26.25% | 3 |
| 26.25% - 30.00% | 3.5 |
| 30.00% - 33.75% | 4 |
| 33.75% - 37.50% | 4.5 |
| 37.50% - 41.25% | 5 |
| 41.25% - 45.00% | 5.5 |
| 45.00% - 48.75% | 6 |
| 48.75% - 52.50% | 6.5 |
| 52.50% - 56.25% | 7 |
| 56.25% - 60.00% | 7.5 |
| 60.00% - 63.75% | 8 |
| 63.75% - 67.50% | 8.5 |
| 67.50% - 71.25% | 9 |
| 71.25% - 100.00% | 9.5 |

MLB - BS load to CIO value mapping
| BS load range | CIO value (dB) |
| --- | --- |
| 0.00% - 63.64% | 0 |
| 63.64% - 72.73% | 0.5 |
| 72.73% - 81.82% | 1 |
| 81.82% - 90.91% | 1.5 |
| 90.91% - 100.00% | 2 |

7. simulation results:
    - 
