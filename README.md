<div align="center"><img src="https://seeklogo.com/images/P/prezi-logo-45485A2255-seeklogo.com.gif"></div>

# CONVEX HULL OF INTERSECTIONS

## :clipboard: Table of Contents
  - [x] [Problem Statement](#problem-statement)
  - [x] [Definitions](#definitions)
  - [x] [Solution](#solution)
  - [x] [Analysis](#analysis)
  - [x] [References](#references)

# PROBLEM STATEMENT 

Write a C++ program that calculates the {+intersections of a set of line segments and circles+}.    
From these intersection points, calculate the {+convex hull and its area+}.  
The program should read from the standard input and write to the standard output.    

## Input
The first line contains a number $`K (0 <= K <= 1000)`$.
The following  K  lines contain the definition of a circle or a line segment:
- If the line starts with "L", there are 4 integers defining the start and end points of the line segment: $`( x_{0}, y_{0}, x_{1}, y_{1})`$.
- If the line starts with "C", there are 3 integers defining the center point and radius of the circle:  $`(x, y, radius)`$.

Note:
- All input numbers are integers, and their absolute value does not exceed 106   .
- The input does not contain any overlapping line segments, nor circles with the same
center and radius.

## Output
The first line contains  **N** , the number of intersecting points.  
The following  **N**  lines contain the coordinates of the intersecting points.    
The next line contains  **H** , the number of points on the convex hull.    
The following  **H**  lines contain the coordinates of point on the convex hull.    
The next line contains  **A**,  the area of the convex hull.  

If all intersection points are collinear, then both **A**    and **H**    should be 0.     
Give your answers rounded to 4 decimal digits.

## Technical requirements
The program should not allocate more than 100 MB of memory, time limit is 3 seconds.   
You can use any C++ standard, but don’t use anything else than the standard libraries.   

| Example input: | Example output: |
| -------- | -------- |
| <p align="left"><img src="https://s26.postimg.org/eoloz4kx5/Screen_Shot_2017-09-16_at_01.07.25.png"></p>   | <p align="left"><img src="https://s26.postimg.org/55c05nxex/Screen_Shot_2017-09-16_at_01.08.25.png"></p>   |
| <p align="left"><img src="https://s26.postimg.org/68w4hmi21/Screen_Shot_2017-09-16_at_01.12.03.png"></p>   | <p align="left"><img src="https://s26.postimg.org/45lp9yi95/Screen_Shot_2017-09-16_at_01.15.11.png"></p>    |

# DEFINITIONS 

This is section intends to provide general knowledge about concepts in the problem for the clearity of algorithmic approaches in [solution](#solution). 

## :small_orange_diamond: Line On the Cartesian Plane

> Lines in a Cartesian plane or, more generally, in affine coordinates, can be described algebraically by linear equations. (Wikipedia)

In two dimensions, the equation for non-vertical lines is often given in the slope-intercept form:
```math
y = mx + b
```
where:

{+m+} is the slope or gradient of the line.   
{+b+} is the y-intercept of the line.    
{+x+} is the independent variable of the function $`y = f(x)`$.

<div align="center"><img src = "https://s26.postimg.org/claq3eeih/line.png"/></div>

The equation of the line passing through two different points $`P_{0}(x_{0}, y_{0})`$ and $`P_{1}(x_{1}, y_{1})`$ may be rewritten as    
```math
(y-y_{0})(x_{1}-x_{0})=(y_{1}-y_{0})(x-x_{0})
```
```math
yx_{1} - yx_{0} - y_{0}x_{1} + y_{0}x_{0} = y_{1}x - y_{1}x_{0} - y_{0}x + y_{0}x_{0}
```

```math
(y_{0} - y_{1})x + (x_{1} - x_{0})y + y_{0}(x_{0} - x_{1}) + x_{1}(y_{1} - y_{0}) = 0
```
Let's consider the equation in the following form:

```math
Ax + By + C = 0
```
Then, it can be derived

```math
A = y_{0} - y_{1}
```
```math
B = x_{1} - x_{0}
```
```math
C = y_{0}(x_{0} - x_{1}) + x_{1}(y_{1} - y_{0})
```
## :small_orange_diamond: Circle

> A circle is a simple closed shape in Euclidean geometry. It is the set of all points in a plane that are at a given distance from a given point, the centre; equivalently it is the curve traced out by a point that moves so that its distance from a given point is constant. (Wikipedia)

<div align="center"><img src="https://s26.postimg.org/obgz7n9ih/circ.png"></div>

```math
(x-a)^2 + (y-b)^2 = r^2
```

## :small_orange_diamond: Convex Hull

> In mathematics, the convex hull or convex envelope of a set X of points in the Euclidean plane or in a Euclidean space (or, more generally, in an affine space over the reals) is the smallest convex set that contains X. (Wikipedia)

<div align="center"><img src="https://s26.postimg.org/mzqdwu3ft/ch26.png"></div>

# SOLUTION 

This section considers step by step solutions of the problem statement dividing the problem into subproblems.

## :small_blue_diamond: Intersection of Two Circles

<div align="center"><img src="https://s26.postimg.org/9e3s0o4qh/circle.png"></div>

Let's start with the calculation of a distance {+d+} between two centers of circles.

```math
d = ||P_{1} - P_{0}||   
```
```math
d = \sqrt{(x_{1} - x_{0})^2 + (y_{1} - y_{0})^2}   
```

There are three cases when an intersection of circles are undefined:    

$`d > r_{0} + r_{1}`$ circles do not intersect    
$`d < |r_{0} + r_{1}|`$ one circle is inside another circle      
$`d = 0, r_{0} = r_{1}`$ circles are coincident thus there are infinite number of intersection points   

Otherwise, let's consider two triangles $`P_{0}P_{2}P_{3}`$ and $`P_{1}P_{2}P_{3}`$ using Pythagoras's theorem:

```math
a^2 + h^2 = r_{0}^2\:\:\:and\:\:\:b^2 + h^2 = r_{1}^2
```

Knowing $`d = a + b`$ we can deriva {+a+} as such

```math
r_{0}^2 - a^2 = r_{1}^2 - b^2
```
```math
r_{0}^2 - r_{1}^2 = a^2 - b^2
```
```math
r_{0}^2 - r_{1}^2 = a^2 - (d-a)^2
```
```math
a = (r_{0}^2 - r_{1}^2 + d^2 ) / (2d)
```
If two circles touch at one point then $`d = r_{0} \pm r_{1}`$

```math
h^2 = r_{0}^2 - a^2
```
```math
P_{2} = P_{0} + a (P_{1} - P_{0})/d
```
Point $`P_{3} = (x_{3}, y_{3})`$ representing by $`P_{0} = (x_{0}, y_{0}), P_{1} = (x_{1}, y_{1})`$ and $`P_{2} = (x_{2}, y_{2})`$   

```math
x_{3} = x_{2} \pm h ( y_{1} - y_{0} ) / d
```
```math
y_{3} = y_{2} \mp h ( x_{1} - x_{0} ) / d
```
```cpp
void find_intersects_circle_to_circle(Circle circle1, Circle circle2) { 
    //Please refer source code
}
```

## :small_blue_diamond: Intersection of Two Lines

<div align="center"><img src="https://s26.postimg.org/fzcx0ii2h/g5382.png"></div>

The equation of line can be represented as coefficients $`A, B, C`$. There are two lines with corresponding coefficients $`A_{0}, B_{0}, C_{0}`$ and $`A_{1}, B_{1}, C_{1}`$. 
If two lines are parallel then there aren't any intersections points. Lets consider following system of linear equations.

<div align="center"><img src="https://s26.postimg.org/i67ywy8l5/formula.png"></div>

Applying Cramer's rule

<div align="center"><img src="https://s26.postimg.org/59q1grri1/g4403.png"></div>

<div align="center"><img src="https://s26.postimg.org/vw2i5qvp5/g4484.png"></div>

If denominator is equal to 0 then lines are parallel 

<div align="center"><img src="https://s26.postimg.org/o3gfrif3t/g4677.png"></div>

```cpp
void find_intersects_line_to_line(Point point1, Point point2, Point point3, Point point4) { 
    //Please refer source code
}
```

## :small_blue_diamond: Intersection of Circle and Line

<div align="center"><img src="https://s26.postimg.org/uqtbq63ft/g4345.png"></div>

Given a circle and a line find intersection points of them. Lets assume that the center of the circle is at $`(0, 0)`$ with radius $`r`$. 
In other case, modify $`C`$ constant of a line equation $`Ax + By + C = 0`$.

Now find the distance from the line closest to the origin $`(x_{0}, y_{0})`$. Let's find the distance

```math
d_{0}=\frac{|C|}{\sqrt(A^2+B^2)}
```
```math
x_{0}=-\frac{AC}{\sqrt(A^2+B^2)}
```
```math
y_{0}=-\frac{BC}{\sqrt(A^2+B^2)}
```
Observing the distance to the origin $`(x_{0}, y_{0})`$ it can be concluded that:

- if distance $`d_{0}`$ is greater that radius $`r`$ then there are intersections.
- if $`d_{0} = r`$ there is exactly one intersection point.
- if $`d_{0} < r`$ there are two intersection points.

Let's find two intersectons points $`(a_{x}, a_{y})`$ and $`(b_{x}, b_{y})`$ for third scenario. It can be concluded that these two points are at the same distance $`d`$ and belongs to $`Ax + By + C = 0`$.

```math
d = \sqrt(r^2 - \frac{C^2}{A^2+B^2})
```
```math
m = \sqrt(\frac{d^2}{A^2+B^2})
```
```math
a_{x} = x_{0} + B * m
```
```math
a_{y} = y_{0} - A * m
```
```math
b_{x} = x_{0} - B * m
```
```math
b_{y} = y_{0} + A * m
```
```cpp
void find_intersects_circle_to_line(Circle circle, Line line) { 
    //Please refer source code
}
```

## :small_blue_diamond: Graham Scan for Convex Hull Construction

Given a set of points construct convex hull that contains all points. For solving this problem I decided to use Graham's method with Andrew's modification. The time complexity of the algorithms is $`O(nlogn)`$.

Lets take the most left and the most right two poins (in case when there are more than two points choose bottom one in left and top one in right points). Thereafter, lets build a line from $`A`$ to $`B`$ and divide the set into two upper and bottom subsets $`S_{1}`$ and $`S_{2}`$. Points $`A`$ to $`B`$ belong to both subsets. Then construct upper $`S_{1}`$ convex and bottom $`S_{2}`$ convex and then merge them to get Convex Hull. In order to get upper $`S_{1}`$ I needed to sort points based on {-x-axis-}. 
In each step I got one point and compare it with two previous neightbours, if these three points are not right rotate then remove nearest neighbour and continue. As result remaining points belong to Convex Hull. The time complexity of $`O(nlogn)`$ is reached during sorting the points.

<div align="center"><img src="https://s26.postimg.org/k0jh9ptyh/convexhull.gif"></div>

```cpp
void convex_hull(vector<Point> &points) {
    //Please refer source code
}
```

## :small_blue_diamond: Convex Hull Area

```math
A = ^1/_2\sum_{i=0}^{n-1} (x_{i}y_{i+1} - x_{i+1}y_{i})
```

```cpp
double convex_hull_area(vector<Point> &points) {
    //Please refer source code
}
```

## :small_blue_diamond: Edge Cases

During collecting intersections there rise cases when several elements intersect at the same point. I had several approaches to solve the issue, finally, choose {-set-} to eliminate duplicates after the collection of data.

<div align="center"><img src="https://s26.postimg.org/dp4y3b7d5/g4317.png"></div>

```cpp
void eliminate_duplicates() { 
    //Please refer source code
}
```

## :small_blue_diamond: Run Against Custom Testcase

### Input

$`7`$             
$`C \,\, 2 \,\, 2 \,\, 3`$     
$`C \,\, 2 \,\, 7 \,\, 2`$    
$`C \,\, 7 \,\, 4 \,\, 4`$    
$`C \,\, 3 \,\, 13 \,\, 1`$       
$`L \,\, -1 \,\, 5 \,\, 3 \,\, -3`$        
$`L \,\, -1 \,\, 5 \,\, 12 \,\, 5`$        
$`L \,\, -2 \,\, -5 \,\, 10 \,\, 10`$       

### Output

$`16`$       
$`-1.0000 \,\,\,\,\,\,\,\,\,\, 5.0000`$      
$`-0.4000 \,\,\,\,\,\,\,\,\,\, 3.8000`$      
$`1.2718  \,\,\,\,\,\,\,\,\,\,\,\,\,-0.9103`$      
$`1.6923  \,\,\,\,\,\,\,\,\,\,\,\,\,-0.3846`$      
$`2.0000  \,\,\,\,\,\,\,\,\,\,\,\,\,-1.0000`$      
$`2.0000  \,\,\,\,\,\,\,\,\,\,\,\,\,\,\,5.0000`$      
$`3.0805  \,\,\,\,\,\,\,\,\,\,\,\,\,\,\,4.7986`$     
$`3.1270 \,\,\,\,\,\,\,\,\,\, \,\,\,\,\,5.0000`$     
$`3.2759  \,\,\,\,\,\,\,\,\,\,\,\,\,\,\,5.4599`$    
$`3.5630 \,\,\,\,\,\,\,\,\,\, \,\,\,\,\,1.9538`$    
$`3.9594 \,\,\,\,\,\,\,\,\,\, \,\,\,\,\,6.5990`$     
$`4.6794  \,\,\,\,\,\,\,\,\,\,\,\,\,\,\,3.3493`$     
$`4.7126 \,\,\,\,\,\,\,\,\,\, \,\,\,\,\,0.7186`$      
$`6.0000 \,\,\,\,\,\,\,\,\,\, \,\,\,\,\,5.0000`$     
$`8.2419  \,\,\,\,\,\,\,\,\,\,\,\,\,\,\,7.8023`$     
$`10.8730 \,\,\,\,\,\,\,\,\,\,\,\,\,5.0000`$     
$`7`$    
$`-1.0000  \,\,\,\,\,\,\,\,\,\, 5.0000`$   
$`1.2718  \,\,\,\,\,\,\,\,\,\,\,\,\,-0.9103`$    
$`2.0000  \,\,\,\,\,\,\,\,\,\,\,\,\,-1.0000`$     
$`4.7126  \,\,\,\,\,\,\,\,\,\,\,\,\,\,0.7186`$     
$`10.8730 \,\,\,\,\,\,\,\,\,\,\,\,5.0000`$     
$`8.2419  \,\,\,\,\,\,\,\,\,\,\,\,\,\,7.8023`$     
$`3.9594  \,\,\,\,\,\,\,\,\,\,\,\,\,\,6.5990`$     
$`55.2580`$    


# ANALYSIS

The calculation of intersection points between two shapes takes constant time $`C`$. Suppose that we have $`n`$ circles and $`n`$ lines. 
The algorithm compares an element with other elements reaching the complexity of $`n^2`$ and for construction of {-Convex Hull-} Graham Scan takes $`nlogn`$. 
The calculation of area of {-Convex Hull-} takes another $`n^2`$.

```cpp
for(int i = 0; i<lines.size(); i++) {
    for(int j = 0; j<circles.size(); j++) {
        //Please refer source code
    }
}
```
```math
Cn^2 + nlogn + n^2 = n^2
```
Time complexity is $`O(n^2)`$

Two shapes intersecting with each other can have intersections at most two points. Traversing collections of shapes gives following number of intersections.

```math
2 (n-1 + n-2 + ... + 1) = 2\frac{n(n-1)}{2} = n(n-1)
```
Space complexity is $`O(n^2)`$

# REFERENCES

[Geometry Algorithms Home](http://geomalgorithms.com)    
[Paul Bourke](http://paulbourke.net)     
[e-maxx](http://e-maxx.ru)     
Other Sources
