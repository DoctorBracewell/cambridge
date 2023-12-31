**Recommended Books**:
- Mathematical methods in the Physical Sciences
- Mathematical Methods for Physics and Engineering
- Advanced Engineering Mathematics
- Mathematical Methods for Science Students

**Scalar** - A quantity with magnitude only
**Vector** - A quantity with magnitude and direction. Vectors are not tied to a particular place in space.
**Unit Vector** - A vector for which the length is 1

**Multiplying by scalar** - The vector length scaled by a constant. Multiplying by a negative scalar reverses the direction of a vector.
Two vectors pointing in the same direction must be related by some constant multiple.
**Adding Vectors** - To add vectors, arrange them into a triangle tip-to-tail and the result is the third side of the triangle.
Commutative, `a + b = b + a` 
Associative, `a + (b + c) = (a + b) + c`
Distributive, `λ(a + b) = λa + λb`
**Subtraction** - Difference of two vectors should be understood as `a + (-b)`.

**Vector coordinates in 2D**  - One way to represent a vector is using `x` and `y` coordinates, this gives the direction and magnitude. `r = (x, y)`
Given an vector and an angle against one of the axes, you can determine the `x` and `y` components of a vector.
**Unit Vectors** - `î = (1, 0)`, `ˆj = (0, 1)
This allows you to do vector algebra **by-component**, adding or multiplying corresponding components would give the result vector in the same form.
**Magnitude of 2D Vector** - `| r | = √(x^2 + y^2)`
**Normalisation** - Dividing all components of a 2D vector by the length will create a **unit vector** in the same direction.

**Vector coordinates in 3D** - Vectors can also exist in 3D space and can be represented with `x`, `y` and `z` components. 
Just as in 2D, 3D vectors can be manipulated by-component, normalised and displaced.
**Unit Vectors** - The units vectors `i, j, k` form a **basis** for 3D space because a combination of them can form any 3D vector.
As long as you have three vectors that are not parallel you can form a basis, there are infinite bases.

**Equation of a line** - `r - a = λ(b - a)` -> `r = a + λb
Express `r` in components, then rearrange and equate the components to find a cartesian equation of the line (in 2D or 3D).

**Scalar Product** - A method of multiplying vectors together. `a • b = |a| |b| cosθ` where without loss of generality `0 <= θ <= π`
- Commutative - `a • b = b • a`
- Distributive - `a • (b + c) = a • b + a • c`
- The square of a vector is its scalar product with itself. `a • a = |a|^2`
- `(a + b)^2 = a^2 + b^2 + 2(a • b)`
- If `a • b = 0` (and `|a|, |b| =/= 0`) then `cosθ = 0` and the vectors are *orthogonal*
- `i, j, k` are often referred to as "orthonormal" because they are orthogonal to each other and have. a length of 1
You can prove the result for two vectors by writing them in component form then distributing the brackets and using the component `i, j, k` orthogonality rules to eliminate terms.

The scalar product can also represent the component of one vector in the direction of another. Ensure that you normalise the vector you are using as the direction of before you calculate the scalar product.

**Planes** - A plane a 3D can be specified with:
- The orientation of the plane, given by a (optionally unit) vector normal to the plane `n`
- A point on the plane with position vector `a`
A general point on the plane has position vector `r`
Therefore the vector `r - a` must lie on the plane, and therefore is orthogonal to `n`
-> `(r - a) • n = 0`

Another property of a plane is the perpendicular distance to the origin.
`p = |a| cos θ` because `p` is the *component of a in the direction of n*, and `n` is a unit vector with a length of one.  Therefore `p = a • ˆn` Therefore `r • ˆn = p`

**Component Formula for Planes** - A plane can also be expressed in component/cartesian form where the co-efficients of `x, y, z` are the normal vector, and this is equal to `a • n`
**Alternative Equation of a Plane** - A plane can also be expressed as a single point and two lines that lie within the plane.

**Equation of a Sphere** - `|r - a| = R` where `a` is the centre and `R` is the radius.
**Equation of a Cylinder** - Consider the component of OR that is parallel to the axis normal to the surface, `|r| cosθ = | r • n|`. This leads to `| r - (r • n)n | = R
**Equation of a Cone** - The property of a cone is that the line from the vertex to any point on the cone always makes an angle `α` with the axis. `(r − q) • n = |r − q| cos α`
![[Pasted image 20231012092420.png]]

**Vector Product** - `a ∧ b ≡ |a| |b| sin θ n`. This finds a normal vector to both vectors `a` and `b`
![[Pasted image 20231012092817.png]]
It is anti-commutative, as the direction of the cross product reverses when swapped. The cross product of two parallel vectors is `0`.

You can write the result in component forms by distributing the brackets and using the component `i, j, k` parallel rules to eliminate terms.

The determinant of the matrix below is the same as the vector product of `a` and `b`.
![[Pasted image 20231012093516.png]]
![[Pasted image 20231012093523.png]]

You can derive alternative equations for lines and planes.
**Line** - `(r − a) ∧ (b − a) = 0` because they must be parallel.
![[Pasted image 20231012094638.png]]
**Plane** - `(b − a) ∧ (c − a)` is the normal to the plane. Therefore `(r - a)` is perpendicular to the normal. Therefore: `(r − a) ⋅ [(b − a) ∧ (c − a)] = 0`.

The two most useful properties is that for **orthogonal vectors the dot-product is `0`**, and for **parallel vectors the cross-product is `0`**.

**Scalar Triple Product** - `a • (b ∧ c) = [a, b, c]`
![[Pasted image 20231017090355.png]]
![[Pasted image 20231017090402.png]]
![[Pasted image 20231017090909.png]]
An even permutation of the arrangement will keep the same sign, an odd one will change the sign.

**Parallelpipeds** - ![[Pasted image 20231017091807.png]]
Area of base = `|b| |c| sin θ = |b ∧ c|`
Height = (component of `a` in direction perpendicular to `b` and `c`) = `a • (b ∧ c)/| b ∧ c |`
-> Volume = `a • (b ∧ c)` = Scalar triple product

**Vector Triple Product** - 
![[Pasted image 20231017092850.png]]
![[Pasted image 20231017092939.png]]

**Vector Area** - Project a planar surface onto the planes `x=0`, `y=0`, `z=0`. Turn the area of these projections into a vector along that axis, then sum these together.
![[Pasted image 20231019192734.png]]

**The vector area of a closed surface is `0`**.
**Vector Area** = Scalar area of the surface * unit vector parallel to surface.

**General Basis Vectors** - Any 3 non-coplanar vectors constitute a basis. If they are all perpendicular the basis is *orthogonal*, if they are also unit vectors the basis is *orthonormal*.

In an orthogonal base, for any vector, you can find the component in one of the directions with
`(u • a) / (a • a) = (u • a) / (|a|^2)`. In the cartesian basis, this simplifies to `(u • a) / 1`

**Cylindrical Polar Coordinates**:
![[Pasted image 20231020214906.png]]
An orthogonal base where a point is represented with `(r, θ, z)`, where `r` represents the radius of the cylinder, `θ` the angle and `z` the height above the cylinder.

Trigonometry can give you components of a cartesian point (`z = z`, simply):
![[Pasted image 20231020215603.png]]
such that
![[Pasted image 20231020215714.png]]
There are also new coordinate directions, these are all *orthonormal*:
![[Pasted image 20231020215832.png]]
![[Pasted image 20231020220321.png]]
However, two of these depend on the *direction* (i.e, `θ`) already being defined, and are not constant like `(i, j, k)`.

Final notes:
![[Pasted image 20231020215920.png]]
![[Pasted image 20231020215941.png]]
^ able to be expressed with just two basis vectors because the value of `θ` is already "included" in the `er` vector

**Plane Polar Coordinates** - Same as cylindrical but without a `z` component.

**Spherical Coordinates**:
![[Pasted image 20231020220639.png]]
An basis where a point is represented with `(r, θ, φ)`, where `r` distance of the point from the origin, `θ` the angle down from the `z` axis (north pole) and `φ` angle along from the `x` axis (`Q->P`) (anticlockwise).

![[Pasted image 20231020221816.png]]
![[Pasted image 20231020221845.png]]
![[Pasted image 20231020221902.png]]

Basis vectors:
![[Pasted image 20231020221948.png]]
![[Pasted image 20231020221956.png]]
and simply:
![[Pasted image 20231020222008.png]]


