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




