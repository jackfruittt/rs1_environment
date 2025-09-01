# rs1_environment

A ROS 2 package providing custom Gazebo Ignition v6.17 simulation environments and models for use in 41068 Robotics Studio 1.

## Overview

This package contains custom worlds, models, and launch configurations for Gazebo Ignition simulations. It includes AprilTags and environmental elements designed for autonomous robot navigation and perception testing.

## Package Structure

```
rs1_environment/
├── config/          # Configuration files (YAML, etc.)
├── launch/          # Python launch scripts for starting simulations
├── models/          # Custom Gazebo models and assets
├── worlds/          # Custom world files (.sdf) for Gazebo Ignition
├── images/          # Photos of the custom world 
├── CMakeLists.txt   # Build configuration
├── package.xml      # Package metadata
└── README.md        # This file
```

## Quick Start

### 1. Build the Package

```bash
cd ~/your_workspace
colcon build --packages-select rs1_environment
source install/setup.bash
```

### 2. Launch a Simulation

```bash
# Launch a basic environment
ros2 launch rs1_environment basic_world.launch.py

# Launch with specific world
ros2 launch rs1_environment custom_world.launch.py world_name:=your_world_name
```

### 3. View in RQT

```bash
# Launch RQT - Check if you're subscribed to image topics for the drone (rs1_drone_n/front/image)
rqt 
```

## Models

### AprilTags
- **Location**: `models/tags/`
- **Families**: tag36h11
- **IDs**: 0-99 available 
- **Usage**: Autonomous detection and localization - respond to "scenario"

### Environment Objects
- **Location**: `models/`
- **Types**: Obstacles, landmarks, terrain features

## Launch Files

The `launch/` directory contains Python launch scripts:

- `spawn_environment.launch.py` : Launches the default world, world_1 

The world argument can be edited to change world selection. 

### Launch Arguments

Common arguments supported by launch files:
- `world`: Specify which world file to load

Example:
```bash
ros2 launch rs1_environment spawn_environment.launch.py world:=world_1 # By Default, it is world_1 anyway
```

## Development

### Adding New Worlds

1. Create your world file in `worlds/`:
   ```xml
   <?xml version="1.0" ?>
   <sdf version="1.9">
     <world name="world_name_goes_here">
       <!-- Your world content and properties -->
     </world>
   </sdf>
   ```

2. Create or edit corresponding launch file in `launch/`:
    - Check launch/ for spawn_environment.launch.py example.
    - Worlds can be added to launch by including in the following section below
            # Start Gazebo to simulate the robot in the chosen world
            world_launch_arg = DeclareLaunchArgument(
            'world',
            default_value='world_1',
            description='Which world to load',
            choices=['mountain_forest', 'test', 'world_1'] # Add more worlds here , seperated by commas
            )

### Adding New Models

1. Create model directory: `models/model_name/`
2. Add required files:
   - `model.sdf`: Model definition
   - `model.config`: Model metadata
   - `materials/`: Textures and materials (For AprilTags, other models have the texture in the meshes directory)

### Creating Custom AprilTags

1. Generate tag images using online generators i.e. using https://chev.me/arucogen/
2. Place converted PNG files in `models/tags/tag_X/materials/textures/`
3. Update albedo map referencing in your AprilTag model SDF files

## Environment/World Photos ##
- World_1 AprilTag cube with the size 3x3x3 compared to 1x1x1 
![AprilTag Cube for scenario representation](</rs1_environment/images/Screenshot%20from%202025-09-01%2019-37-20.png>)
![AprilTag Cube for scenario representation](</rs1_environment/images/Screenshot from 2025-09-01 19-05-57.png>)

- World_1 Top_Down View
![Map View of World_1](</rs1_environment/images/Screenshot from 2025-09-01 18-11-17.png>)
 
## Troubleshooting

### Common Issues

**Gazebo Mesh not Found**
- Verify mesh files if it is valid and not corrupt, re-export mesh file into the meshes directory of the model 

**Gazebo Doesn't load texture**
- Verify texture for the mesh is in the same directory or is mapped properly in Blender.
- Import the exported .dae mesh to verify if texture is properly mapped to mesh and loads.
- Ensure file name referencing are correct in .dae - check using VsCode 

**Gazebo Doesn't Run / is Frozen**
- Reset Gazebo GUI

## Acknowledgments
- AprilTag library by University of Michigan and AprilTag generation using https://chev.me/arucogen/
- Gazebo Ignition simulation framework
- ROS 2 community and 41068 Ignition Bringup Package from 41068 RS1 2025 Spring