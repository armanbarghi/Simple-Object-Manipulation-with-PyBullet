# Simple-Object-Manipulation-with-PyBullet

## URDF Generation from .glb Models for PyBullet

This script pipeline prepares URDF models for PyBullet simulation using `.glb` files as input.

### Why `.glb`?

- `.obj` export with relative textures from Blender failed due to viewport shading crash — possibly GPU-related.
- `.glb` contains both geometry and textures in a single file, avoiding Blender’s export issues.

### Conversion Process

1. **Convert `.glb` to `.obj`**  
   We convert each `.glb` file into `.obj` using `trimesh`, applying a rotation for alignment.

2. **Material Handling**  
   PyBullet only supports one `map_Kd` texture per `.obj`. Multi-textured models must be split:
   - Each material group is exported as a separate `.obj` file.
   - These parts are linked in a single `.urdf` file.

3. **Collision Handling**  
   PyBullet assumes convex meshes by default. To improve collision accuracy:
   - We run [VHACD](https://github.com/kmammou/v-hacd) to generate convex decompositions.
   - The decomposed `.obj` file is used for collision geometry in all URDF parts.

## 🎯 2D Bounding Box Projection in PyBullet

This module computes 2D bounding boxes of 3D objects in a PyBullet simulation using camera projection.

### 🔍 How It Works

- **Mesh Sampling**: A set of surface points is randomly sampled from the object's `.obj` mesh file.
- **World Transformation**: These points are transformed into world coordinates using the object's position and orientation from PyBullet.
- **Camera Projection**: The transformed points are projected into 2D image coordinates using the camera's view and projection matrices.
- **Bounding Box Generation**: A 2D bounding box is computed by taking the min/max of the projected pixel coordinates.

This approach enables automatic annotation of 3D objects in 2D camera views, ideal for generating training data for object detection tasks.
