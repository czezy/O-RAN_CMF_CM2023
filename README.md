# O-RAN_CMF
Conflict Mitigation Framework for O-RAN

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


7. simulation results:
