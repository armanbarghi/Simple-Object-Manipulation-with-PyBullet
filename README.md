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
