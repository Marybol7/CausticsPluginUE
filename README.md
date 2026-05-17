# LiquidFX – Real-Time Liquid & Caustics Plugin for Unreal Engine 5

![Unreal Engine](https://img.shields.io/badge/Unreal%20Engine-5.x-black?style=for-the-badge&logo=unrealengine)
![Blueprints](https://img.shields.io/badge/Blueprints-Visual%20Scripting-blue?style=for-the-badge)
![Materials](https://img.shields.io/badge/Materials%20%26%20Shaders-Real--Time%20Graphics-purple?style=for-the-badge)
![Niagara](https://img.shields.io/badge/Niagara-VFX-orange?style=for-the-badge)

**LiquidFX** is a modular Unreal Engine 5 plugin developed as my Bachelor's Thesis project.

It focuses on the real-time simulation of liquid effects, including animated water materials, caustics, wave deformation, underwater post-processing, foam, ripples, splashes and simplified buoyancy.

The goal of this project is not to create a physically accurate fluid simulation, but to build a flexible and visually convincing real-time system using Unreal Engine materials, Blueprints, Niagara and post-processing.

---

## Demo

Project website:  
https://marybol7.github.io/CausticsPluginUE/

> Recommended: add a short GIF or video here showing the final demo level.

```md
![LiquidFX Demo](Docs/Images/liquidfx_demo.gif)
```

---

## Overview

LiquidFX is designed as a reusable plugin rather than a single isolated scene.

The system is built in layers, where each visual module can be adjusted or extended independently. The project combines shader-like material logic, Blueprint interaction, Niagara particles and post-processing to create a complete liquid effects system for real-time scenes.

Main areas covered by the project:

- Real-time water material with animated normals.
- Vertex displacement using sine and Gerstner waves.
- Caustics using both light projection and animated plane materials.
- Depth Fade and depth-based color blending.
- Underwater post-process material with distortion and progressive darkening.
- Foam generation for shoreline/contact areas and wave crests.
- Impact ripples generated on the water surface.
- Niagara-based splash effects.
- Simplified buoyancy for floating objects.
- Different liquid presets and material instances, such as water, oil, honey and tar.

---

## Features

### Water Material

The base water material uses animated normal maps to create a first layer of surface movement.

It exposes parameters such as color, opacity, normal intensity, tiling and movement speed, allowing the material to be adjusted from instances without modifying the master material.

### Wave Deformation

The surface can be deformed using World Position Offset.

The system includes:

- Sine wave deformation.
- Gerstner wave deformation.
- A blend between sine and Gerstner waves.
- Noise-based variation to reduce repetitive patterns.

This makes it possible to create different water behaviours, from calm surfaces to more pronounced wave motion.

### Caustics

LiquidFX includes two approaches for caustics:

- **Spotlight caustics**, where an animated caustics material is projected through a light.
- **Plane-based caustics**, where an animated material is placed directly over a surface.

Both techniques are visual approximations intended for real-time use.

### Depth Integration

The water material includes visual integration effects such as:

- **Depth Fade**, used to soften intersections between water and geometry.
- **Depth-based color**, used to make shallow areas appear lighter and deeper areas appear darker.

These effects help the liquid feel more connected to the environment.

### Underwater Scene

The underwater setup combines several effects:

- Suspended particles using Niagara.
- Screen-space distortion using a post-process material.
- Progressive darkening based on depth.
- Distance-based visibility loss, making distant objects darker than nearby ones.

This creates a more immersive underwater atmosphere without relying on expensive fluid simulation.

### Foam

Foam is generated using two main approaches:

- **Shoreline foam**, based on contact/proximity with surrounding geometry.
- **Height-based foam**, used to emphasize wave crests.

Noise, threshold, softness and intensity parameters are used to avoid a uniform or artificial result.

### Object Interaction

The plugin includes a first layer of water-object interaction:

- Circular ripples generated at the impact position.
- Niagara splashes spawned when objects touch the liquid.
- Simplified buoyancy based on water height, object depth and damping.

This helps the liquid react visually and physically to objects in the scene.

### Multiple Liquid Types

The system can be adapted to different liquids through material instances and Blueprint presets.

The current demo includes:

| Liquid | Visual Approach |
|---|---|
| Water | Transparent, blue, lighter movement |
| Oil | Smoother and slightly denser appearance |
| Honey | Warmer, thicker and slower-looking |
| Tar | Dark, opaque and visually heavy |

---

## Technical Highlights

This project focuses on real-time graphics and technical art inside Unreal Engine.

Some of the main technical topics explored are:

- Material graph development.
- Shader-like logic using Unreal Engine materials.
- World Position Offset for vertex animation.
- Gerstner and sine wave implementation.
- Scene Depth usage for water integration.
- Post-process materials for underwater effects.
- Blueprint-to-material communication.
- Dynamic material instances.
- Niagara VFX integration.
- Simplified physics-based interaction.
- Plugin-oriented project organization.

---

## Plugin Structure

The plugin is organized into several main folders:

```txt
LiquidFX/
├── Blueprints/
│   ├── Actors/
│   ├── Controller/
│   ├── Interaction/
│   └── Types/
├── Demo/
├── Materials/
│   ├── Masters/
│   └── Instances/
├── Niagara/
└── Textures/
```

### Main folders

- **Blueprints**: actors and logic for liquid planes, interaction and floating objects.
- **Materials**: master materials and material instances for water, caustics, underwater effects and liquid variants.
- **Niagara**: particle systems used for splashes and underwater particles.
- **Textures**: normal maps, noise textures and caustics textures.
- **Demo**: test scenes used to showcase the plugin modules.

---

## How to Use

1. Clone or download the repository.
2. Open the project in Unreal Engine 5.
3. Make sure the required plugins, such as Niagara, are enabled.
4. Open one of the demo levels.
5. Place a LiquidFX water/liquid actor in the scene.
6. Assign one of the provided material instances.
7. Adjust exposed parameters such as:
   - color,
   - opacity,
   - normal intensity,
   - wave amplitude,
   - Gerstner influence,
   - foam intensity,
   - caustics intensity,
   - liquid density,
   - buoyancy and damping.
8. Add an interactable object to test ripples, splashes and buoyancy.

> Note: the system is designed as a visual real-time approximation, not as a full fluid simulation.

---

## Performance

A small performance test was carried out in the demo scene using Unreal Engine profiling tools such as `stat fps`, `stat unit` and `stat gpu`.

Example test hardware:

```txt
CPU: Intel Core i7-10700KF
GPU: NVIDIA RTX 3060
RAM: 32 GB
Resolution: 1920x1080
```

Example results:

| Configuration | FPS | Frame Time | GPU Time |
|---|---:|---:|---:|
| Scene without water | 59.91 | 17.57 ms | 15.92 ms |
| Base material only | 57.66 | 17.13 ms | 15.47 ms |
| + Sine wave | 53.17 | 18.63 ms | 17.01 ms |
| + Gerstner waves | 52.02 | 19.22 ms | 17.75 ms |
| + Noise variation | 51.44 | 19.30 ms | 17.68 ms |
| Full surface system | 48.30 | 20.72 ms | 19.02 ms |
| Full system + underwater | 40.71 | 24.41 ms | 22.43 ms |

The most expensive part of the system is the underwater setup, mainly because post-processing affects the final rendered image.

The modular structure allows individual effects to be adjusted or disabled depending on the needs of each scene.

---

## Screenshots

> Recommended folder structure:

```txt
Docs/
└── Images/
    ├── water_base.png
    ├── caustics_comparison.png
    ├── waves_comparison.png
    ├── underwater.png
    ├── foam_ripples.png
    └── liquid_types.png
```

### Water and Caustics

![Water and Caustics](Docs/Images/caustics_comparison.png)

### Wave Systems

![Wave Systems](Docs/Images/waves_comparison.png)

### Underwater Scene

![Underwater](Docs/Images/underwater.png)

### Foam and Interaction

![Foam and Interaction](Docs/Images/foam_ripples.png)

### Liquid Variants

![Liquid Variants](Docs/Images/liquid_types.png)

---

## What I Learned

This project allowed me to explore real-time graphics from both a technical and practical perspective.

The main areas I worked on include:

- Building modular materials in Unreal Engine.
- Translating mathematical wave models into material graphs.
- Working with World Position Offset and Scene Depth.
- Creating post-process effects for underwater environments.
- Connecting Blueprints, materials and Niagara systems.
- Designing a plugin structure intended for reuse.
- Balancing visual quality and real-time performance.

---

## Limitations

LiquidFX is not a physically accurate fluid simulation.

The system uses visual and simplified physical approximations to achieve a convincing real-time result.

Current limitations include:

- Limited support for multiple simultaneous ripple impacts.
- Simplified buoyancy model.
- No physically accurate viscosity simulation.
- Caustics are texture-based approximations.
- Visual quality depends on mesh density, lighting and material parameters.

---

## Future Work

Possible future improvements include:

- Render Target based ripple accumulation.
- More advanced buoyancy using multiple sampling points.
- Better viscosity behaviour for different liquids.
- More optimized underwater post-processing.
- Improved plugin UI for easier parameter control.
- Additional liquid presets.
- More complete documentation for public release.

---

## Academic Context

This project was developed as part of my Bachelor's Thesis in Computer Engineering.

It is focused on real-time graphics, shader/material development and Unreal Engine plugin design.

---

## License

This project is currently shared as an academic and portfolio project.

A formal license will be added if the plugin is prepared for public reuse or distribution.
