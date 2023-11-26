**Name:** Brace Godfrey
**CRSId:** dg681
# Warmup Questions
### 1
> What is OpenGL?

OpenGL is an open-source API and pipeline for rendering graphics on a GPU. It includes a rendering pipeline that includes some programmable stages, where a programmer can write "shaders" that affect the rendering process in GLSL, an OpenGL-specific programming language.
### 2
> How is Vulkan different from OpenGL?

Vulkan is an alternative to OpenGL. It aims to achieve the same result, but has an API with a much lower overhead - more work and code is required from the programmer to implement the rendering pipeline, but Vulkan also offers much finer-grained control over how exactly pixels are rendered to the screen as well as better performance.
### 3
> Put the the following stages of the OpenGL rendering pipeline in the correct order. Very briefly explain what each stage does and comment whether each stage is programmable.

1. **Primitive Setup** - In this non-programmable stage OpenGL sets up the rendering pipeline, such as creating the object scene, establishing a data connection with the GPU or compiling shaders.
2. **Vertex Shader** - In this programmable stage OpenGL runs a shader over all defined vertices in the scene. This might calculate/change the normal or position of the point and pass it onto future stages.
3. **Clipping** - In this non-programmable stage OpenGL runs some sort of clipping algorithm to handle occlusions, where one object obscures some part of another. Parts of the hidden object should not be projected to the scene because they are blocked by an object closer to the camera. This could be achieved by e.g. a z-buffer algorithm.
4. **Rasterisation** - In this non-programmable stage OpenGL converts objects into collections of triangles, initialising each vertex of the triangle using the previous vertex shader. It also performs interpolation of all points within the triangle, creating a fragment for each pixel that needs to be drawn to the screen.
5. **Fragment Shader** - In this programmable stage OpenGL runs a shader over each fragment (i.e, pixel) in each triangle. The programmer can use this shader to calculate the colour of the pixel using passed-in properties such as intersection point positions, normal vectors, object colour, light sources, etc. Most of these values are calculated by the interpolation of rasterised triangles rather than the properties of the true point on the object in the scene.
### 4
> Search for "normal map" images on the internet. Why do they tend have an overall blue shade?

A "normal map" is a texture file that defines how points on an object should be "bumped" around. Instead of defining the colour of a pixel, the texture defines how the normal vector of the point that the texel is mapped to should be adjusted.
Normal maps tend to be predominantly blue because they are stored as a 3D vector, where the `z` coordinate is set to some value e.g. `1` that represents how the normal is perpendicular to the surface. The `x` and `y` coordinates are used to adjust the normal, and often these are only small numbers. When this vector is saved as a texture file and interpreted as an image, the `z` represents the blue channel and is often much larger than the red (`x`) or green (`y`) channels.
# Main Questions
### 1
> Describe the z buffering algorithm. Compare the projection matrix on slide 86 with the projection matrix in the 2010P4Q4 past paper, and discuss which one you need to use for Z buffering.
> 
> Slide 86 matrix - ![[Pasted image 20231123211348.png]]
> 
> Past Paper matrix - 
> ![[Pasted image 20231123211529.png]]

The z-buffer algorithm is used to implement occlusions in the rendering process, where one object that is closer to the camera blocks the view of all or some part of another object. 
The z-buffer implements this by using a depth buffer which is originally set to the farthest `z` coordinate that the camera can see to, and a colour buffer that is originally set to the background colour. For each fragment of each triangle in the scene, calculate/retrieve the `z` value of the associated point. If it is smaller/closer than the depth buffer than update the colour buffer of that fragment and set the depth buffer to the new, closer `z` value. Make sure to reset the buffers when moving on to the new fragment.

Question: does this mean that different triangles in the scene can have the "same" fragment? Does "fragment" refer to the actual `(x, y)` coordinates of the pixel to be rendered?

The matrix from the 2010 past paper cannot be used to implement the z-buffer algorithm as using it to project a point actually removes the `z` component - no information about it is saved. In contrast, the matrix from slide 86 projects a point and keeps some information about the `z` value of the original point. It should be noted that the value is transformed to `1/z`, though, so the algorithm requires slight adjustments.
### 2
> What is the worst case scenario, in terms of a number of times a pixel colour is computed, when rendering N triangles using the Z-buffer algorithm? How could we avoid such a worst-case scenario?

The worst case would be `n` times, if the algorithm loops through the triangles in farthest-to-nearest order. If the depth buffer is originally set to the farthest point the camera could see, then in each subsequent triangle the point for the fragment `(x, y)` would have a *closer* depth than the current buffer, and hence the colour would be computed. Then the algorithm moves onto the next, closer triangle and the depth for the same point `(x, y)` is closer so the colour is recomputed, and the previous colour discarded.
Finally the algorithm reaches the closest triangle and calculates the colour for that point, which becomes the actual fragment colour.

This could be avoided by sorting triangles in a scene, such that closer ones are visited by the algorithm first. However this may add additional computation time to the rendering.
### 3
> How could you use the following texture types to texture a sphere in OpenGL?

> a) 2D

A 2D texture could be mapped to a sphere using `uv` coordinates, where each defined vertex of the sphere has a `uv` texture coordinate associated with it, and then these coordinates are interpolated throughout the triangles formed by OpenGL to create the sphere 

> b) 3D

I am unsure exactly how 3D textures work in OpenGl. If they work just as 2D textures but with an additional dimension, then `uvw` coordinates could be used.

> c) CUBE_MAP

A CUBE_MAP texture could be used for environment mapping in OpenGL. Often this is done as a cube for simplicity, but a sphere can be used as well. The texture is mapped onto the inside of a sphere surrounding the scene. 
### 4
> For downsampling an image, **briefly** explain how each of the following sampling techniques work (feel free to use khronos.org when unsure). Find or generate some illustrations of typical artefacts where relevant. Discuss performance, storage and visual quality.

Question: I am not 100% sure what "nearest" vs. "linear" means when downsampling. I would guess that "nearest" uses the the most common texel value in the sampled area (mode), whereas linear takes the average from all texels in the sampled area (mean)?
I've also found it quite difficult to find examples of nearest neighbour vs linear interpolation for *downsampling*, as they seem to be more common when upsampling?

- `GL_NEAREST` - This is a sampling method that doesn't use a MipMap. When it used for downsampling (where multiple texels map to a single pixel), the nearest neighbour of the sampled area is used). At lower resolutions this can look "crisper" than linear sampling.
- `GL_LINEAR` - This sampling method doesn't use a MipMap. When it is used for downsampling, it takes a linear average from the texels to calculate the pixel value. At higher resolutions this can look smoother than nearest-neighbour sampling.

A MipMap is when the same texture is stored at multiple resolutions. This is useful as the texel sampling is pre-computed and can improve rendering performance, but it does an require an additional 1/3 of storage for the texture file.

- `GL_LINEAR_MIPMAP_NEAREST` - This sampling method uses the closest MipMap and performs a linear interpolation sample over the area from that MipMap. The MipMap can improve performance, but the quality is the same as linear sampling and the MipMap requires more storage.
- `GL_LINEAR_MIPMAP_NEAREST` - This sampling method uses the closest MipMap and performs a linear interpolation sample over the area from that MipMap. The MipMap can improve performance, but the quality is the same as linear sampling and the MipMap requires more storage.
- `GL_NEAREST_MIPMAP_LINEAR` - This sampling method samples over multiple MipMap levels, and performs a nearest neighbour sample over those areas, which are then linearly blended together to produce the result. The MipMap can improve performance, and the quality may look even better for resolutions that do not exactly match a MipMap resolution, but the MipMap requires extra storage.
- `GL_LINEAR_MIPMAP_LINEAR` - This sampling method samples over multiple MipMap levels, and performs a linear interpolation sample over those areas, which are then linearly blended together to produce the result. The MipMap can improve performance, and the quality may look even better for resolutions that do not exactly match a MipMap resolution, but the MipMap requires extra storage.
# Questions on colour & perception
### 1
> What is an image? How are digital images represented in memory?

An image is a two-dimensional array of pixels, each with an associated colour value. These are often represented as 2D arrays.
### 2
> What is quantisation?

Quantisation is the process of mapping some continuous value to a discrete one, often used in digital signal processing. For colour displays, it is used to turn all of the colours in an image into a finite number, with the intent of preserving the original image as best as possible.
### 3
> What is colour banding?

When only a few colours are used in an image, the lines where the pixels switch from one colour to another can stand out significantly. The effect of this is often furthered by the Mach bands illusion. It can be helped with dithering, where there is an area of pixels between the two solid-colour areas, where some pixels are one colour and some the other, which leads to a smoother-looking gradient.
### 4
> What is the difference between luma and luminance?

Luminance is a measure of light, defined by the quantity of light emitted from a source or received by an observer. 

Luma is gamma-corrected luminance, and represents the brightness of an image in image processing.
### 5
> Why is gamma correction needed?

The gamma-correction process leads to a more perceptually uniform scale of brightness to the human eye compared to the luminance scale.
### 6
> What are the differences between rods and cones?

Cones are light-sensitive cells used for daylight-vision and colour perception, whereas rods are also light-sensitive cells but are used for night-vision. The fovea has a very high concentration of cones but no rods, and the sides of the retina have a high concentration of rods that decreases the further away from the fovea but no cones.
### 7
> How can two colour spectra appear the same? What are these called then?

When two colour spectra appear the same they are called metamers. This is possible because even though there are an infinite number of wavelengths, cone cells only have 3 types of photoreceptors - short, medium and long. Some wavelength can have the same combination of these as some other wavelength (I think?)
### 8
> What is the relation between LMS cone sensitivities, CIE XYZ and the RGB space of a monitor?

Cone sensitives relate to how human cone cells have 3 types of photoreceptors (short, medium and long) which have different probabilities of whether or not they will pick up light of a particular wavelength.
CIE XYZ is an abstract colour space defined in terms of `x`, `y`, `z` variables (all of which are between 0 and 1), and is intended to link to perceived colours in human vision. All channels in the CIE XYZ colour function are *positive*, and the overall space is intended to represent all possible colours the human eye can see.

(Question: is there some defined method or transformation between LMS cone sensitivites and CIE channels?)

The RGB space of a monitor or display is the space of all colours that a particular monitor can display. It tends to differ between manufacturers and models, so CIE XYZ was established as a device-independent model. Pixels in the RGB space are given in terms of red, green and blue channels. 

(Question: how exactly does CIE XYZ relate to RGB spaces of particular monitors? are most monitors capable of displaying all of CIE, or do they define some are of it they can display etc. etc?)
### 9
> Explain the purpose of tone-mapping and display-encoding steps in a rendering pipeline.

Slight unsure as this work was submitted before the lecture on tone mapping, but best-guess answer below:

Tone mapping involves adjusting the luminance of pixels in an image. It is often used to reduce dynamic range so that smaller-range devices can display the image as accurately as possible, customise the appearance of an image with specific filters or simulate particular visions.
### 10
> What is the rationale behind sigmoidal tone-curves?

Unsure.