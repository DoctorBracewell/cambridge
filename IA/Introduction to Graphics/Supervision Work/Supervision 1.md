**Name:** Brace Godfrey
**CRSId:** dg681
# Warmup Questions
### 1
> Watch NVIDIA's real-time [ray tracing demo](https://www.youtube.com/watch?v=NgcYLIvlp_k) from 2020 to get in the mood.

Done :D
### 2
> What are the ray parameters of the intersection points between ray (1,1,1) + t(−1,−1,−1) and the sphere centred at the origin with radius 1?

`t = 1 +/- (sqrt(3) / 3)`
### 3
> Why do we need anti-aliasing?

Aliasing occurs when rays passing through the centre of the pixel cause artefacts such as jagged edges of objects or missing objects entirely, because the single ray is not enough accurately represent the object. Anti-aliasing is a variety of techniques used to limit the effect of aliasing, to more accurately ray-trace the objects onto a grid of pixels. It can include different sampling types or distributed ray tracing.
### 4
> What is the difference between a point light source and an area light source?

A point light source shines light only from a single point, and rays are drawn exactly to that point. An area light source created using distributed ray tracing, and involves rays being drawn to multiple points over a certain area. An area light source can create softer shadows than a point light source.
### 5
> We use a lot of triangles to approximate stuff in computer graphics. Why are they good? Why are they bad? Can you think of any alternatives?

Triangles are often used for rasterization, when surfaces are split up into polygons so they can be efficiently rendered. Triangles are useful for this as they are a basic shape and any other polygon can be built built out of them. Curved surfaces can also be approximated to a particular accuracy depending on how many triangles can be used. Each vertex of the triangle has a number of attributes associated with it and this can be used to interpolate values within the triangle to be projected to the screen.
In theory any polygon could be used, but this may limit how many shapes you could represent - if you split the surfaces up into pentagons, it would be difficult to accurately represent a square, for example.
# Longer questions
### 2017 Paper 4 Question 3
> What is meant by the following terms? Explain how their contribution to the overall amount of reflected light is calculated.

> i) Ambient illumination

Ambient illumination is the light on a specific point given the background illumination of the scene and the colour of the object at that point. It does not take into account any other light sources in the scene. It can be calculated by multiplying the colour of the object by the background illumination.

> ii) Diffuse reflection

Diffuse reflection is when light rays are scattered at many angles. It can be calculated by, for each light source in the scene, multiplying the light intensity by the diffuse coefficient and the diffuse reflection intensity, and the cosine of the light source to the normal of the surface.

> iii) Specular Reflection

Specular reflection is the bouncing of one light ray off of a surface in one direction. It can be calculated by multiplying the light intensity by the specular coefficient, and multiplying the vector of perfect reflection by this to "bounce" the light beam off of the object at the point. It is raised to the roughness coefficient as well to approximate imperfect specular reflection. 
### 2
> What information would you need to define the volume of space that might be visible from the camera during rendering (research view frustum if in doubt)?

To define the volume of space visible from the camera, you would need a few values:
- The farthest `z` value that can be seen by the camera. 
- The `z` coordinate of the viewing plane (the 2D plane that the scene is projected onto)
- The coordinates and orientation of the camera (if not already at origin)
- A field of view angle in the `y` direction.
### 3
> Write pseudo-code for the ray tracing algorithm.

```
for each pixel:
	cast ray through pixel centre
	
	for each object in scene
		if ray intersects object
			if current intersection is closer than last intersection
				calculate illumination (using each light source in scene)
				set pixel background to illumination at point
		else 
			set pixel to background illumination
```
### 4
> Explain how Ray tracing can achieve the following effects:

> i) Reflections

Reflections can be achieved using recursively "bouncing" specular reflection rays between objects. A set number of bounces can be defined at the start, and then Phong's illuminations model calculated for each point and then bounced to next point after reflecting the ray.

> ii) Refractions

Refractions can be calculated by bending light rays as they pass through a transparent object. Objects may have a refractive index that determines how much the ray is bent by.

> iii) Shadows

Rays are being traced from the object to the light, so if the ray encounters another object on the way there it will be casting a shadow on the original object. Self-shadowing can also occur where an object shadows parts of itself.
### 5
> Provide two examples for distributed ray tracing and explain how the selected techniques works

1. One example of distributed ray tracing is for area lights. If one point on the objects sends multiple points to some area, this models there being a light source spread out over that whole area. This distributed ray tracing can lead to much softer shadows.
2. In an animated scene, temporal distributed ray tracing can be used to achieve motion blur. Sending multiple rays towards an object as it moves/translates its position will give a motion blur in the scene.
### 6
> Describe the Model, View, and Projection transformations. Comment on why we use homogeneous co-ordinates.

**Model** - The model transformation is used to take an object in its local coordinates and transform it into world coordinates so it is correctly scaled, positioned and oriented. 

**View** - The view transformation is used to ensure the camera is positioned at the origin and oriented correctly, to simplify later transformations such as the projection. Typically it translates and rotates the entire environment to position the camera rather than the camera individually.

**Projection** - The projection transformations turns all the points of objects in the 3D environment into points on a 2D plane for the image. It is often used in conjunction with an occlusion algorithm such as a z-buffer. Performing a view transformation so that the camera is at the origin and oriented a specific way helps simplify the projection transformation.

Homogenous coordinates are used so that 3D translations can be represented using matrix multiplications. Usually the coordinates are normalised so that `w = 1` for simplicity.
### 7
> When transforming objects into world co-ordinates using matrix `M`, position vectors are pre-multiplied with `M`. Discuss whether this matrix is suitable to transform the objects' normals. If not, can you suggest an alternative?

This is not necessarily suitable to transform the objects' normal vectors, because the objects may be transformed "non-orthogonally" (question: what exactly does this mean?) which does not preserve the angles between the the surface and the normal vector.

Instead you should consider the relationship between the normal vector and tangent vector. Let:
- `T'` be the transformed tangent vector, so `T' = MT`
- `N'` be the transformed normal vector, so `N' = GT` where we are trying to find matrix `G` that tells us how to transform the normal vectors.
These must be perpendicular with each other before and after the transformation, so 
`N • T = 0` and 
`N' • T' = 0`, hence `GN • MT = 0`. 
Given `T`, `N` and `M` you can derive transformation `G = (M^-1) transpose`, and use this as the transformation for the objects' normal vectors.
### 2010 Paper 4 Question 4
> Homogeneous coordinates are often used to represent transformations in 3D:
> ![[Pasted image 20231117225308.png]]

> i) Explain how to convert standard 3D coordinates, `(x, y, z)`, to homogeneous coordinates, and how to convert homogeneous coordinates to standard 3D coordinates.

To convert 3D coordinates to homogenous coordinates you must choose a particular (non-zero) value of `w` (as any set of 3D coordinates maps to an infinite number of homogenous coordinates distinguishes by their `w` value). 
Then just multiply the `x`, `y`, `z` components by this `w` value and these become the `xH`, `yH`, `zH` components of your homogenous coordinates, and add the `w` value as a fourth component.

To convert back to 3D coordinates, just divide the `xH`, `yH`, `zH` homogenous coordinates by the associated `w` value and remove the `w` component from the vector.

> ii) Describe the types of transformations provided by each of the four blocks of coefficients in the matrix `(a11 . . . a33, b1 . . . b3, c1 . . . c3 and d)`.

The `a` coefficients can be used to represent any standard 3D transformation that can be represented by a 3x3 matrix - scales, rotations, shears, etc.

The `b` coefficients can be used to represent translations of the `x`, `y`, `z` coordinates. 

I am unsure what the `c` coefficients might be used for. Perhaps translating the `w` value, although I am unsure when this would ever be useful as I understand the `w` value *only* as a scaling value.

The `d` coefficient only affects the `w` value, so it can be used for a scaling transformation as an alternative to a scale represented in the upper-left 3x3 portion of the matrix without changing the actual `x`, `y`, `z` values. Changing the `w` value of homogenous coordinates will scale the point.

> iii) Explain what transformation is produced by each of the following matrices:

> a) 
> ![[Pasted image 20231117230847.png]]

I am still unsure what exactly the effect of the `c` coefficients are, but experimentation shows that this "copies" the `z` value to the `w` value. This might be used in a projection of some kind, as this would have the effect of scaling the point depending on how close it is to the origin (i.e, the camera). It would make further objects look further away.

> b) 
> ![[Pasted image 20231117230856.png]]

I am totally unsure what this matrix might do.

> Consider the following figure: ![[Pasted image 20231117231836.png]]

> a) Give a matrix, or product of matrices, that will transform the square `ABCD` into the rectangle `A'B'C'D'`.

![[Pasted image 20231117233503.png]]

> b) Show what happens if the same transformation is applied to `A'B'C'D'`.

![[Pasted image 20231117233945.png]]]]