# O-RAN_CMF
Conflict Mitigation Framework for O-RAN

## Simulation scenario
1. base stations and simulation area:
    - 19 base stations (BS) distributed on a hexagonal grid
    - distance between each BS is equal to 600 meters
    - simulation area is limited with a polygonal shapre approximating coverage distance of the network
    - coverage provided by each BS is not split into sectors

    ![Simulation area and distributed base stations](/figures/base_stations.png)
    
2. users:
    - 380 randomly distributed across the simulation area (20 users per base station)
    - each user has a randomly assigned user profile deciding his demanded bitrate (user profiles defined below)
    - 80% of users are pedestrians, 20% of users are moving in vehicles
    - users move randomly around the simulation area with constant movement speed (5 m/s for pedestrians, 25 m/s for vehicles)
    - user position is updated each 0.05 s of simulation time
    - movement direction is changed on each position update with probability of 0.06% or when area boundary is reached with probability of 100%

    ![Considered user distribution](/figures/users.png)

3. user profiles:
    - each profile defines user's demanded bitrate, i.e., the bitrate that user requires from the network to suffice for his data throughput requirements
    - the user profiles are assigned at random, with prespecified probabilities
    - three user profiles:
        - low bitrate: 96 kbps, 60% probability (green color)
        - medium bitrate: 5000 kbps, 30% probability (yellow color)
        - high bitrate: 24000 kbps, 10% probability (red color)

4. traffic generation:
5. RIC configuration:
