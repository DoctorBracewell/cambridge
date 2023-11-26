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

Projection Assumptions:
- Screen centre at `(0, 0, d)
- Screen parallel to xy plane
- `z`-axis into screen
- `y`-axis up, `x`-axis right
- camera at origin

However camera can be moved to any pointer with new equations, or the *whole environment* can be shifted to ensure the camera remains at the origin.
### Modelling Pipeline
![[Pasted image 20231116001308.png]]^ This is a *useful* and *common* method of 3D dimensions into computer graphics, but not hard and fast model on how things should be done.

**Modelling Transform** - Turning an object into the correct coordinates relative to the environment.
E.G take a model (teapot) with its origin at centre, rotate and place it correctly in the world. Differenet for each object.
![[Pasted image 20231116001758.png]]

**View Transform** - Position all objects relative to the camera. This could be done by moving the camera to the origin and using more complex mathematics, but more commonly is done by moving and rotating the entire environment such that the camera is positioned at the origin and oriented the correct way.
![[Pasted image 20231116002015.png]]

**Projection Transform** - Turn 3D points into 2D points. Relatively simple after the modelling and view transformations because camera is at origin and oriented simply.
![[Pasted image 20231116002344.png]]

All together:
![[Pasted image 20231116002729.png]]

**Finding view transformations**:
![[Pasted image 20231116002935.png]]
Suppose you have two vectors describing the camera:
- `c->l`, which shows where the camera is pointing
- `u`, which shows which direction is "up" for the camera.
It is possible to find the orthogonal basis vectors (`r`, `u`, `v`) that ensure point `c` (the camera) becomes the "origin" of the new space.

For a left-handed coordinate system:
- ![[Pasted image 20231116003330.png]]
- ![[Pasted image 20231116003343.png]]
- ![[Pasted image 20231116003459.png]]

**Using view transformations**:
First you want to *translate* so that the camera becomes at the origin:
![[Pasted image 20231116003607.png]]

then apply a *change of basis* transformation
![[Pasted image 20231116003638.png]]

So that `V` becomes:
![[Pasted image 20231116003935.png]]

**Transforming Normal Vectors**:
![[Pasted image 20231116004628.png]]
![[Pasted image 20231116004450.png]]
`G` is derivable as below:
	![[Pasted image 20231116005039.png]]
Or can just be known as: `( M^-1 ) transposed

**Scene Graph**
A scene can be built from a scene graph, where each node inherits all of its parents transformations and its own transformations.
![[Pasted image 20231116010323.png]]
![[Pasted image 20231116010408.png]]
### Rasterization
![[Pasted image 20231116010657.png]]

Drawing polygons with uniform shading gives poor result. If you calculate the colour at each *vertex* of the triangle and interpolate throughout the triangle you get much smoother colours.
The efficiency of rasterization comes from the ability to interpolate vertex attributes, including colour, normals, texture, coordinates, etc.

**Homogenous Barycentric Coordinates** can be used to achieve this.
![[Pasted image 20231116011319.png]]
Each vertex one of `α`, `β`, `γ` is `1`, and each vertex has some attribute (here it is an RGB value).
At a vertex inside the triangle, the attribute is built up proportionally from each coordinate value:
![[Pasted image 20231116011616.png]]

You can visualise barycentric coordinates with the diagram below, and find `(x, y)` in barycentric coordinates given the coordinates of the vertices of the triangle.
For example, at any point along the line connecting `a` and `b`, `γ = 0` because only at `c` is it 1.
![[Pasted image 20231116011757.png]]
![[Pasted image 20231116012719.png]]

Pseudocode for triangle rasterization interpolation:
![[Pasted image 20231116012908.png]]
If all barycentric coordinates are greater than `0` then point inside triangle:
![[Pasted image 20231116013009.png]]

A variety of optimisations:
- Starting and ending only from pixels on the line in the triangle
- Barycentric coordinates change by the same amount when moving one pixel right, precompute increments.

You can also use this to interpolate *normal vectors* between each vertex in the triangle, used for Phong's illumination model. To calculate colour for each pixel, add the ambient illumination to the diffuse term and the specular term, but the normal can be interpolated for either of the diffuse or specular terms that require it.
This is only really relevant when the normal vectors are pointing different directions, i.e. on a surface that is not flat. These normals must be calculated from the *actual model* of the surface not on the triangle vertices, as obviously all vertices would have the same normal vector.

**Occlusions** - When one object hides part of another. This can be a difficult problem to solve because some triangles may overlap only part of another.
**Z-Buffer** - An algorithm to implement occlusions.
Start with a colour buffer set to the background colour and a depth buffer set to the farthest point, then:
![[Pasted image 20231116111408.png]]

Z-Buffer must store depth with sufficient precision, usually 24 or 32 bit integers or floats.
![[Pasted image 20231116111616.png]]
**Z-Fighting** - When two polygons are essentially on top of each other and can be drawn differently per frame.
### Graphics Hardware and OpenGL
**GPU** - Graphics Processing Unit, optimised for floating point operations on large arrays of data. 
Uses include:
- Clipping, rasterisation, surface removal
- Procedural shading, texturing, animation, simulation
- Ray tracing
- Video rendering and decoding
- Physics engines
- Modern ones tend to be fully programmable
- Optimised for massively parallel operations

3D rendering can be efficiently *parallelised*. Modern GPUs contain hundreds or thousands of SIMD processors, and beyond 1000 GB/s memory access.

**GPU APIs**:
OpenGL
- Multi-platform
- Open standard API
- Focus on general 3D applications
- no ray tracing
DirectX:
- Windows/Xbox only
- Proprietary API
- Focus on games
Vulkan:
- Cross-platform
- Open standard
- Low-overhead API for high performance
- Compared to OpenGl/DirectX it reduces CPU load, finer GPU control, better multi-core support
- But very complicated
Metal:
- Low level, low-overhead 3D GFX and compute shaders API
- Support for apple chips, intel, AMD
- Mostly used on iOS

OpenGL and DirectX are not designed for general purpose computing, so
CUDA:
- Proprietary language for parallel computing on the GPU
- C-like programming language
- Special API for parallel instructions
- Requires NVIDIA Gpu
OpenCL:
- Same organisation as OpenGL
- Cross platform, most GPUs support it.

OpenGL ES for lower performance systems (mobile phones)
WebGL for websites, essentially a wrapper around OpenGL ES

**Java Libraries**:
- LWJGL 3 for OpenGL access alongside OpenCL
- JOML for linear algebra operations, designed for OpenGL use
### OpenGL Rendering Pipeline
**CPU-Side**:
- `gl*` functions that:
	- create OpenGL objects
	- copy data CPU <-> GPU
	- modify OpenGL state
	- enqueue operations
	- synchronise CPU & GPU
- C99 library
- wrappers in most programming language

**GPU-Side**:
- Fragment shaders
- Vertex shaders
- other shaders
- written in GLSL
	- Similar to C
	- can be written in other language and compiled to SPIR-V

![[Pasted image 20231116114120.png]]

**Vertex Shader** - Process vertices, normals, UV texture coordinates

**Tessellation Control Shader** - You can choose the subdivision level of tessellation
![[Pasted image 20231116114154.png]]

**Geometry Shader** - Operate on tessellated geometry, you can create new primitives.
![[Pasted image 20231116114300.png]]

**Fragment Shader** - Computes colour per each fragment. Can lookup colour in the texture, modify pixel's depth values, tone mapping, etc. etc. Most computation happens here.
![[Pasted image 20231116114442.png]]

**Preparing vertex data**:
For a cube
	![[Pasted image 20231116114656.png]]


Prepare two arrays:
- All vertices with their attributes (colour, normal, etc.)
- Groups of 3 vertices that represent all triangles (usually specified in counter-clockwise order, to help OpenGL determine which triangles are facing the camera)
![[Pasted image 20231116114747.png]]
### GLSL Fundamentals
**Shader** - A small program executed on the GPU. Executed for each vertex, each pixel, etc.
**OpenGL Shading Language (GLSL)** - A language used to write shaders, similar to C/Java

**Example**:
![[Pasted image 20231116115522.png]]

**Swizzling** - In GLSL, extracting some number of components from an aggregate type into a smaller one, e.g. `Vec4(...).xyz` creates a `Vec3`, or even change the order.

**Shader Inputs & Outputs** - 
![[Pasted image 20231121162652.png]]

**OpenGL Application Flow** -
![[Pasted image 20231121163513.png]]

For example, some key components of drawing an object:
- `glUseProgram` to activate vertex and fragment shaders
- `glVertexAttribPointer` to indicate which vertex and normal buffers should be passed to the shader
- `glUniform*` to set parameters of the vertex/fragment shaders
- `glBindTexture`, `glBindVertexArray` to bind resources
- `glDrawElement` to run the shader and draw the element.
### Textures
Types of textures:
![[Pasted image 20231121163807.png]]
**2D** are most common.
**Texel** - A pixel in a texture.

**Texture Mapping**:
- Define the texture image
- Let `(u, v)` be coordinates between `0` and `1` on the texture (for ease of use as they are same regardless of texture resolution).
- For each vertex of each triangle on your rasterised object, specify the `(u, v)` coordinates of the texture.
- Interpolate `(u, v)` for each fragment in the triangle using barycentric, then lookup the correct colour for that point from the texture file.

**Sampling**:
Up Sampling, when the object is larger than the texture and a single texel is mapped to multiple pixels. This can be done either with nearest neighbour or bilinear (x and y axis) interpolation.

For more complex textures, nearest neighbour can lead to blocky artefacts. The only real way to avoid any artefact at all is to use a sufficiently high resolution texture.
![[Pasted image 20231121164436.png]]

Down Sampling, when multiple texels map to a single pixel. Makes it necessary to sample texels or average texture across an area.

**MipMap** - Textures are often stored at multiple resolutions as a mipmap. Each texture is half the size of the lower level. This actually only takes 1/3 more memory to store, as in the diagram below. 
This is useful as it provides "pre-filtered" textures, where the average over a particular area is already calculated. This makes it much more efficient when down-sampling.

![[Pasted image 20231121164643.png]]

Down-sampling example:
![[Pasted image 20231121165009.png]]

**Texture Tiling** - Texture folds over such that ![[Pasted image 20231121165048.png]]

**Bump/Normal Mapping** - A special kind of texture that modified the *normal* vector for specific points. The surface is still flat, but shading appears as an uneven surface.
![[Pasted image 20231121165143.png]]
Normal mapping is when the actual `xyz` components are stored, bump mapping stored how the normal is changed.

**Displacement Mapping** - Actually effects the geometry of the surface, creating extra triangles etc. etc to provide a more accurate 

**Environment Mapping** - Mapping texture to an environment around the object. Often implemented as a cube:
![[Pasted image 20231121165657.png]]

**Texture Summary in OpenGL** -
![[Pasted image 20231121165804.png]]

**Sampler** - Defines how texels are looked up. Takes a texture file/buffer/memory and seperates the application of the texture attributes so it can be reused multiple times.
### Raster Buffers
GPU's have a few buffers used in the rendering process

**Front** - Used by the GPU to draw to screen
**Back** - Set by a shader to the currently rendered

**Depth** - Used to resolve occlusions
**Stencil** - Specific to OpenGL, can be used to "block" certain pixels from being drawn. E.G. drawing pixels in a mirror and then changing the stencil to draw all pixels outside of the mirror.

Front and back buffers are used for **double buffering**, to avoid flickering, tearing, etc.
![[Pasted image 20231121170336.png]]
Back buffer only sent to frame once drawing is complete. This is done by "swapping" pointers to each buffer.

Might even use **triple buffering** for extra efficiency (see in above image how there are gaps where no rendering is being done)
![[Pasted image 20231121170439.png]]
### Vertical Synchronisation (V-Sync)
Pixels are copied to the monitor *row-by-row*. However, if buffers are swapped during this time you can get a tearing artefact.
![[Pasted image 20231121170619.png]]
You can activate v-sync to ensure that the buffers aren't swapped until this is finished.
![[Pasted image 20231121170722.png]]
However this can lead to lag if frame rendering takes longer than the frame refresh rate (causes stutter). V-Sync trades off between frame lag and tearing artefacts.

**Variable Refresh Rate** - When the GPU controls timing of the frames on the display. Basically tells the monitor how many frames it can do, and they try to synchronise together.
G-Sync (NVIDIA)
FreeSync (AMD)
![[Pasted image 20231121170905.png]]
### Human Vision and Colour
**The eye** - 
- The retina is an array of light detection cells
- The fovea is the high resolution area of the retina
- The optic nerve takes signals from the retina to the visual cortex
- The cornea and lens focus light onto the retina
- The pupil shrinks and expands to control the amount of light.

Two classes of photoreceptors
- Cones are responsible for day-light vision and colour perception.
- Rods are responsible for night vision.
- ![[Pasted image 20231123111412.png]]

**Light & The Electromagnetic Spectrum** -
- Visible light is electromagnetic waves in the range 380nm to 730nm.
- Earth's atmosphere lets in a lot of light in this wavelength band.
- Higher in energy than thermal infrared, so heat does not interfere with vision.

Colour is the result of our perception.
For emissive displays, `colour = perception(spectral_emission)`.
For reflective displays, `colour = perception(illumination x reflectance)`

Most light we see is reflected from objects, which absorb a certain part of the light spectrum.
Same objects may appear to have different colours under different illuminations.

**Colour Vision** - There are three types of cones:
- **S**hort, sensitive to short wavelengths
- **M**edium
- **L**ong
A sensitive curve is the probability that photons of that wavelength will be absorbed.
![[Pasted image 20231123112318.png]]

![[Pasted image 20231123112414.png]]

**Metamers** - Even if two light spectra are different they may appear to have the same colour. These are known as metamers.

Metamers are important, because displays might not emit same light spectra as real-world but they look the same.

**Tristimulus Colour Representation** - Any colour can be matched using three linear independent reference colours. However this may required "negative" contribution from some channel.
![[Pasted image 20231123112824.png]]
^ This is known as a **colour function**, and describes how the 3 basis colours can be used to create a variety of wavelengths.

**Standard Colour Space** - CIE-XYZ, an organisation/standard for use of colours (printing, display, accessibility, etc.)

CIE Experiments, 1931:
- Colour matching experiments
- Group ~12 people with normal colour vision
- Basis for CIE XYZ 1931 CM functions.

CIE 2006 XYZ:
- Derived from LMS colour matching functions
- S-cone response is the most different.

CIE-XYZ Colour Space Goals:
- Abstract from concrete primaries used in experiments
- All matching functions are positive
![[Pasted image 20231123113218.png]]

**Chromaticity Diagram** - 
![[Pasted image 20231123113244.png]]
![[Pasted image 20231123113253.png]]
- Pure colours lie on outer curve
- All other colours are a mix of pure colours
- Points outside the curve do not "exist" as colours.

**Achromatics** - 
![[Pasted image 20231123113733.png]]

**Luminance** - Measure of light weighted by the response of the achromatic mechanisms. Measured in `cd/m^2` or `nit`.
![[Pasted image 20231123113820.png]]

**Visible vs displayable colours** -
- All physically possible and visible colours form a solid in XYZ space.
- Each display device can reproduce a subspace of that space.
- A chromaticity diagram is a slice of this shape.
- The solid is known as a **colour gamut**. 

**HDR** - High dynamic range, attempts to capture/represent almost all visible colours.
**SDR** - Standard dynamic range, attempts to capture only colours of a standard sRGB colour gamut (mimicking CRT monitors).

**Gamma Encoding** - Used to encode luminance or tristimulus colour values in imaging systems.
![[Pasted image 20231123114516.png]]

**Luma** - Pixel brightness in gamme corrected units. 
![[Pasted image 20231123114706.png]]
The gamma-corrected pixel values give a scale of brightness that is more perceptually uniform.

**Transforming between RGB colour spaces** -
![[Pasted image 20231123115302.png]]
e.g.
![[Pasted image 20231123115317.png]]

