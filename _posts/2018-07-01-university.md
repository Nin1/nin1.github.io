---
title: University - LOD & Tessellation
flair: C++/OpenGL
date: 2018-07-01 12:00:00 +0000
icon: assets/thumbnails/2021/uni.png
alttext: Some work I did
categories: [Work, University]
tags: [university, graphics, game engine, OpenGL, C++] # TAG names should always be lowercase
portfolio: true
hidden: true
---

I studied Games Programming at Bournemouth University from 2014 - 2018, and my final year project was focused on level-of-detail (LOD). LOD is the name for a group of techniques used to optimise the amount of computation done when rendering an object based on its importance - usually this 'importance' heuristic is determined by an object's size on-screen, which in turn is determined by its distance from the camera.

By applying LOD techniques well, you can draw a much more detailed scene than otherwise possible. I implemented a number of techniques in my own game engine, written in OpenGL and C++ - You can find the source code [here](https://github.com/Nin1/university-game-engine).

<iframe width="480" height="270" src="https://www.youtube.com/embed/8CELh37Eg4g" frameborder="0" allowfullscreen></iframe>

---

# Mesh-centric methods

### Mesh-swapping

Mesh-swapping is simply swapping to a mesh with fewer triangles when an object is small on-screen. You can use multiple meshes for multiple levels-of-detail, and intelligently select the mesh to use with a cost/benefit algorithm to optimise how much geometry is drawn across the whole scene. To avoid 'popping' when the mesh is swapped, I used a dithering algorithm to smoothly transition between models - This does mean two models are drawn at once briefly, but it's a nice effect!

<iframe width="360" height="270" src="https://www.youtube.com/embed/-HVj5Z_1PUs" frameborder="0" allowfullscreen></iframe>
<iframe width="360" height="270" src="https://www.youtube.com/embed/o7WYfg3EjNk" frameborder="0" allowfullscreen></iframe>

### 2D Billboards

For very distant objects, mesh-swapping can be taken to an extreme by replacing the mesh with a 2D plane showing a texture of the object instead. This works well for roughly-cylindrical objects such as trees, but in the case of the [Stanford Dragon model](https://en.wikipedia.org/wiki/Stanford_dragon) I rendered one texture for each of 8 directions and selected an appropriate one. The texture can react appropriately to lighting using a normal map.

![billboards]({{site.url}}/assets/posts/2018-07-01-university/billboards.png)

---

# Tesselation-centric methods

Tessellation is hardware-accelerated geometry sub-division, which can then be manipulated to add detail or reduce the jaggedness of models. In the OpenGL pipeline, this occurs after the vertex shader but before the fragment shader. The amount of sub-division can be adapted to optimise GPU load in a continuous manner, rather than in discrete steps like mesh-swapping.

### Displacement Mapping

Tessellation is hardware-accelerated geometry sub-division, which can then be coupled with a 2D 'displacement map' to add extra detail to flat geometry. In my demo, I used it on a flat plane to add a 3D stone texture to the ground. Regarding LOD, the tessellation amount and displacement magnitude can be dropped off as the object becomes smaller on-screen to smoothly reduce the amount of detail shown as objects move into the distance.

### Mesh-smoothing & Silhouette tessellation

Using a technique called 'Phong tessellation' a low-poly mesh can be sub-divided, and the jagged edges smoothed, to improve the quality of meshes closer to the camera. As an optimisation, if the game is shaded using Phong lighting, then only the silhouette of the model needs to be tessellated in order for the whole model to appear smooth! This was a very satisfying effect to have working.

<iframe width="480" height="270" src="https://www.youtube.com/embed/UsNVOaIBYuU" frameborder="0" allowfullscreen></iframe>

![silhouette]({{site.url}}/assets/posts/2018-07-01-university/silhouette.png)
