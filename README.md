

# AOV Relight Toolkit
A Nuke gizmo for AOV-based relighting in compositing. Adds virtual lights to rendered scenes using Normal, Position, and Albedo passes.

![AOVRelight2_point](https://github.com/user-attachments/assets/a4cbee7c-a638-45fe-8190-77ba03170348)
![AOVRelight2_directional](https://github.com/user-attachments/assets/7f8d78ee-d32a-4f58-810a-50ac1a898397)
![AOVRelight2_Compmode](https://github.com/user-attachments/assets/96679d73-4071-4185-9091-51e27b81d9cd)

## Requirements
- Nuke 13+ (tested on Nuke 15)
- EXR with the following AOVs:
  - `N` : World-space Normals
  - `P` : World-space Position
  - `DiffuseFilter` : Albedo
  - `DiffuseLighting` : Diffuse light pass (for Replace Lighting mode)
  - `SpecularLighting` : Specular light pass (for Replace Lighting mode)

## Installation

1. Copy `AOV_Relight.gizmo` to your `.nuke` folder 
2. Add to `menu.py`:
```python
toolbar = nuke.menu('Nodes')
toolbar.addCommand('Filter/AOV_Relight', 'nuke.createNode("AOV_Relight")')
```
3. Restart Nuke

### Light Tab

<img width="537" height="300" alt="image" src="https://github.com/user-attachments/assets/7c3fbe47-7c08-4199-a71e-ea22376971c9" />

| Parameter | Description |
|-----------|-------------|
| Light Type | Directional (parallel rays) or Point (radial, distance-based) |
| Light Direction | Direction vector for Directional mode |
| Light Position | World-space position for Point mode |
| Light Color | RGBA color picker |
| Intensity | Light brightness multiplier |
| Ambient | Minimum fill light (prevents pure black shadows) |
| Wrap | Softens the light terminator (0 = sharp Lambert, 1 = hemisphere) |

### Specular Tab

<img width="537" height="300" alt="image" src="https://github.com/user-attachments/assets/2dfa0712-43d8-4ead-8083-0ef0c0109e6e" />

| Parameter | Description |
|-----------|-------------|
| Enable Specular | Toggle specular calculation on/off |
| Spec Intensity | Specular brightness multiplier |
| Shininess | Highlight sharpness (8 = broad, 256 = pinpoint) |
| Fresnel Strength | Edge specular boost (Schlick approximation, inspired by [Alex Villabon](https://youtu.be/FZo0dckEklc)) |

### Options Tab

<img width="537" height="300" alt="image" src="https://github.com/user-attachments/assets/d4b44116-6c71-4932-9074-1b68f1a43efd" />

| Parameter | Description |
|-----------|-------------|
| Camera Position | World-space camera position (needed for specular) |
| Auto-Detect from EXR | Reads camera transform from Redshift EXR metadata |
| **Comp Mode** | |
| Add to Beauty | Original render + new light added on top |
| Replace Lighting | Original diffuse & specular removed, replaced with new light |
| Relight Only | Pure kernel output (debug/isolation view) |
| Source | Original beauty passthrough |

### Camera Auto-Detection
- Reads `exr/rs/camera/transform` metadata from Redshift EXR files. Camera position is extracted from the translation component (indices 12, 13, 14) of the 4x4 transform matrix. 
- #### ** only Redshift renderer supported at the moment 

## Author

Hyunsu Yang
- Portfolio: [hyunsuyang.artstation.com](https://hyunsuyang.artstation.com)
- Email: hyang.vfx@gmail.com

## Version History

- **v1.0** — Initial release: Directional/Point light, Blinn-Phong specular, Fresnel, distance attenuation, camera auto-detect, 4 comp modes
