### Background
![[Pasted image 20231102110603.png]]
**Computer Graphics** - Displaying images, 2D and 3D, on a computer screen to imitate real life.
All visual computer imagery uses computer graphics, and a large amount of other visual imagery (CGI, VR/AR, books/magazines etc.)

**Image** -
- Computing perspective (discrete): a 2D array of pixels, with colour data. Representable in memory and editable in image processing software
- Mathematical perspective (continious): a 2D function.

Images can be stored as a 2D array of pixels. In most cases, each pixel takes 3 bytes (RGB). This can be stored in memory **row-major** or **column-major**. 
Colour values can be stored **interleaved** or **planar**
![[Pasted image 20231102112135.png]]

**Pixel Indices**:
row-major - `i(x, y) = x + y * cols
column-major - `i(x, y) = x * row + y`
interleaved row-major - `i(x, y, c) = 3 * x + 3 * y * cols + c`

**Stride** - A scalar that indicates how to move between each data (e.g. how to move from one x co-ordinate to the next).
![[Pasted image 20231102112413.png]]

Sometimes an image can be "padded" with extra pixels, or to define a region of interest.
![[Pasted image 20231102112451.png]]

**Pixel** - A picture element, usually consisting of three values describing red, green, blue.
These values are often in the 0-255 range because one byte is used to store them.

**Grayscale** - Single colour channel, 1 byte
**Highcolor** - Two colour channels, 2 bytes
**Truecolor** - Three colour channels, 3 bytes
**Deepcolor** - Even more channels, >= 4 bytes

If there are not enough bits to represent colour you get "colour banding", which looks worse because of Mach band. This can be helped with dithering (added noise).
![[Pasted image 20231102113227.png]]

An image can also be seen as a function `I(x, y)` that gives an *intensity value* for any given coordinates. This can then be sampled to a rectangle.
In mathematics, a pixel:
- has no dimension
- occupies no area
- cannot be seen

**Sampling** - Process of mapping continuous function to a discrete one.
**Quantisation** - Process of mapping a continuous variable to a discrete one.
![[Pasted image 20231102113644.png]]

### Rendering
There are many *depth cues* to our brains that can be used to trick us into see 3D objects in a 2D image.
![[Pasted image 20231102113921.png]]

**Ray Tracing**:
- Identify a camera or eye
- Given a set of 3D objects, shoot a ray from the eye through the centre of every pixel and see what surface it hits. Whatever the ray hits determine the colour of that pixel.
	![[Pasted image 20231102114915.png]]

Ray tracing easily handles reflection, refraction, shadows and blur, but is computationally expensive.
![[Pasted image 20231102115120.png]]

**Intersections of rays with objects**:
ray: `P = O + sD`, s >= 0

plane: `P • N + d = 0`
-> 
`s = (O•N) + d / N • D 
polygon or disc: check plane intersection as above, then see if intersection lies within polygon.

sphere: `(P - C) • (P - C) - r^2 = 0`
->
![[Pasted image 20231107111218.png]]
^ Gives quadratic equation, if imaginary roots then no intersection, if multiple roots use closest point of intersection.
### Shading
Once you have found the intersection, you can calculate the normal to the object at that intersection point and shoot rays from that point to all the light sources. Calculate diffusion and specular reflection, will give the colour of the object at that point.

![[Pasted image 20231107111709.png]]

In this way raytracing is almost the "opposite" of the way our eyes see, as the rays go from `camera` -> `object` -> `light source`.

You can also check if there is an object in between the intersection and light source and hence calculate shadows.
![[Pasted image 20231107111749.png]]

**Refraction** - Objects can have transparency, and light rays will be refracted (bent) as it goes through the object. 

**Reflection** - If a surface is totally or partially reflective then new rays can be spawned to find the contribution to the pixel's colour given by the reflection. This is known as "perfect" or "mirror" reflection.
The new rays can be calculated by "reflecting" the direction of the incoming ray vector, e.g. 
`R = - V + 2N(V • N)` 

![[Pasted image 20231107112722.png]]

A surface can also absorb some wavelengths of light, which gives "shiny" colours. Specular reflection can have a "self-shadowing" effect from bouncing rays on the imperfect object surface.

**Diffuse Shading**:
Assuming:
- Only diffuse shading
- All light comes only from a light source
- No object casts a shadow
- Light sources are considered infinitely apart (vector to the light is the same across the whole surface)
![[Pasted image 20231107113921.png]]
where:
- `L` is a normalised vector in the direction of the light source
- `N` is the normal to the surface
- `Il` is the intensity of the light source
- `Kd` is the proportion of the light which is diffusely reflected by the surface
- `I` is the intensity of the light reflected by the surface.

**Imperfect Specular Reflection**:
Phong developed an approximation to specular reflection.
![[Pasted image 20231107113635.png]]
Where:
- `L` is a normalised vector in the direction of the light source
- `R` is the vector of perfect reflection
- `N` is the normal to the surface
- `V` is a normalised vector pointing at the viewer
- `Il` is the intensity of the light source
- `ks` is the proportion of the light that is specularly reflected by the surface
- `n` is Phong's "roughness" coefficient
- `I` is the intensity of the specularly reflected light.

The overall shading equation can be given by the 
**ambient illumination** + **diffusion shading** + **specular reflection**
![[Pasted image 20231107114029.png]]
^ Calculated for each colour channel.
### Sampling & Aliasing
So far assumed that each ray passes through the centre of pixel, but this can lead to:
- jagged edges to objects
- small objects being missed completely
- thin objects being split
![[Pasted image 20231107114552.png]]

These artefacts are known as **aliasing**. **Anti-aliasing** are methods to reduce the effects of this.

**Single Point** - Shoot a single ray through the centre of a pixel
**Regular Super-Sampling** - Shoot multiple rays at regular distances through the pixel and average the result.
**Random Super-Sampling** - Shoot rays at random points through the pixel and average the result.
**Poisson Disc Super-Sampling** - Shoot `N` rays at random points through the pixel such that all rays are at least `ε` distance from each other (hard to implement).
**Jittered Super-Sampling** - Divide pixel into `N` sub-pixels and shoot a ray at a random point in each pixel, designed to approximate poisson disc sampling.
**Adaptive Super-Sampling** - Shoot a few rays, check variance, then decide to continue if dissimilar.

Super-sampling is one reason to take multiple samples per pixel. Another reason is to apply **distributed ray tracing**. This can be achieved either by:
1. Each ray per pixel is allocated a random value from the relevant distributions.
2. Each ray spawns multiple rays when it hits an object. 

Reasons for/examples of distributed ray tracing include:
- Distribute samples over an area (for anti-aliasing)
	![[Pasted image 20231107115508.png]]
	
- Distribute rays going to a light source over some area (produces soft shadows)
	![[Pasted image 20231107115524.png]]

- Distribute camera position over some area (allows simulation of camera with actual-sized lens)
	![[Pasted image 20231107115539.png]]

- Distribute samples over time (produces motion blur or exposure).
## Rasterisation
- Unfortunately ray tracing is very computationally expensive, tends to only be used for super high visual quality.
- Most real-time applications use rasterisation:
	- model surfaces as polyhedra
	- use composition to build scenes
	- apply perspective transformations onto 2d camera screen
	- work out which surface was closest
	- fill pixels with colour of nearest polygon

**Thee-Dimensional Objects** - Polyhedral surfaces are made up from meshes of multiple connected polygons. Polygon meshes can be open or closed, and curve surfaces can be *approximated*.
![[Pasted image 20231109111423.png]]

**Triangles** - Best polygon to use:
![[Pasted image 20231109111700.png]]
### 2D Transformations
**2D Transformations** - Transform predefined points to arbitrary sizes, locations and orientations.
![[Pasted image 20231109111836.png]]

Transformations can be represented using matrix multiplications:
![[Pasted image 20231109112319.png]]
To find out a particular matrix for a transformation, consider how the transformations move the points `(0, 1)` and `(1, 0)`.

**Homogeneous Coordinates** - Translations cannot be represented using simple 2D matrix multiplication, so we switch to *homogeneous* 2D coordinates.
![[Pasted image 20231109112643.png]]

An infinite number of homogeneous coordinates map to every 2D point, e.g
`(1, 2, 1) = (2, 4, 2)`. When `w = 0` this is a point at infinity. Typically assume the inverse transform to be `w = 1`.
![[Pasted image 20231109112905.png]]

and to translate:
![[Pasted image 20231109112940.png]]

Transformations can be concatenated by applying the first matrix to the point and the rest to the result of the last transformation (not commutative).

To transform around an arbitrary point:
- Translate by `(-x, -y)` to the origin
- Apply transformation around the origin
- Translate back to `(x, y)`.

Homogeneous coordinates can also be used for 3D transformations.
Transformations can be used to display multiple instances of an object defined at one location.
### 3D-2D Projection
To make a picture, the 3D world is *projected* onto a 2D plane.

**Parallel Projection** - Removing an axis, useful in CAD/architecture etc. to represent top-down, side, birds-eye views etc. Unrealistic.
**Perspective** - Things get smaller as they are further away, similar to how eyes work. Realistic.
![[Pasted image 20231109115358.png]]

This projection can be represented as a matrix operation
![[Pasted image 20231109115609.png]]