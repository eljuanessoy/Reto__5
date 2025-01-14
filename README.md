# Reto__5
By Juan Esteban Molina Rey (eljuanessoy)

### 1. Create a package with all code of class Shape, this exersice should be conducted in two ways:
### A unique module inside of package Shape

```css
estructura_archivos/
├── shapes/
│   ├── __init__.py
│   ├── shapi.py
└── main.py
```

```python
shapi.py

import math

class Point:
    def __init__(self, x: int, y: int):
        self._x = x
        self._y = y

    def get_x(self):
        return self._x

    def set_x(self, value):
        self._x = value

    def get_y(self):
        return self._y

    def set_y(self, value):
        self._y = value

    def compute_distance(self, other_point: 'Point'):
        return ((other_point.get_x() - self.get_x()) ** 2 + (other_point.get_y() - self.get_y()) ** 2) ** 0.5


class Line:
    def __init__(self, start_point: Point, end_point: Point):
        self.start_point = start_point
        self.end_point = end_point
    
    def length(self):
        calcu = self.start_point.compute_distance(self.end_point)
        return calcu

class Shape:
    is_Regular = True

    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        self.vertices = vertices
        self.edges = edges
        self.inner_angles = inner_angles

    def compute_area(self):
        pass
    
    def compute_perimeter(self):
        return sum(edge.length() for edge in self.edges)

    def compute_inner_angles(self):
        count = len(self.vertices)
        return (count-2) * 180
        

class Rectangle(Shape):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 4:
            raise ValueError("A rectangle must have 4 vertices")

    def compute_area(self):
        length1 = self.edges[0].length()
        length2 = self.edges[1].length()
        return length1 * length2

class Square(Rectangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)       
        if len(vertices) != 4:
            raise ValueError("A Square must have 3 vertices")
        if self.edges[0].length() != self.edges[1].length():
            raise ValueError("A square must have equal sides")


class Triangle(Shape):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
    def area_triangle(self):
        s=0.5*self.compute_perimeter()
        return math.sqrt(s*(s-self.edges[0].length())*(s-self.edges[1].length())*(s-self.edges[2].length()))
    
class EquilateralTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not (self.edges[0].length() == self.edges[1].length() == self.edges[2].length()):
            raise ValueError("An Equilateral triangle must have three equal sides")
        
class IsoscelesTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not ((self.edges[0].length() == self.edges[1].length() != self.edges[2].length()) or
                (self.edges[0].length() == self.edges[2].length() != self.edges[1].length()) or
                (self.edges[1].length() == self.edges[2].length() != self.edges[0].length())):
            raise ValueError("An Isosceles triangle must have two equal sides")
        
class ScalenTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not ((self.edges[0].length() != self.edges[1].length() != self.edges[2].length())):
            raise ValueError("A Scalene triangle must have three different sides")
```

``` python
main.py

from shapes.shapi.py import *

vertex1 = Point(0, 0)
vertex2 = Point(0, 1)
vertex3 = Point(1, 1)
vertex4 = Point(1, 0)

line1 = Line(vertex1, vertex2)
line2 = Line(vertex2, vertex3)
line3 = Line(vertex3, vertex4)
line4 = Line(vertex4, vertex1)

rectangle = Square([vertex1, vertex2, vertex3, vertex4], [line1, line2, line3, line4], [90, 90, 90, 90])

print("Perímetro del rectángulo:", rectangle.compute_perimeter())

print("Área del rectángulo:", rectangle.compute_area())
print(rectangle.compute_inner_angles())

triangle_vertex1 = Point(0, 0)
triangle_vertex2 = Point(3, 0)
triangle_vertex3 = Point(1.5, 3 * math.sqrt(3) / 2)  

triangle_line1 = Line(triangle_vertex1, triangle_vertex2)
triangle_line2 = Line(triangle_vertex2, triangle_vertex3)
triangle_line3 = Line(triangle_vertex3, triangle_vertex1)

equilateral_triangle = EquilateralTriangle([triangle_vertex1, triangle_vertex2, triangle_vertex3],[triangle_line1, triangle_line2, triangle_line3], [60, 60, 60])

isosceles_vertex1 = Point(0, 0)
isosceles_vertex2 = Point(3, 0)
isosceles_vertex3 = Point(1.5, 2)  

isosceles_line1 = Line(isosceles_vertex1, isosceles_vertex2)
isosceles_line2 = Line(isosceles_vertex2, isosceles_vertex3)
isosceles_line3 = Line(isosceles_vertex3, isosceles_vertex1)

isosceles_triangle = IsoscelesTriangle([isosceles_vertex1, isosceles_vertex2, isosceles_vertex3], [isosceles_line1, isosceles_line2, isosceles_line3], [60, 60, 60])

scalene_vertex1 = Point(0, 0)
scalene_vertex2 = Point(3, 0)
scalene_vertex3 = Point(2, 4)  

scalene_line1 = Line(scalene_vertex1, scalene_vertex2)
scalene_line2 = Line(scalene_vertex2, scalene_vertex3)
scalene_line3 = Line(scalene_vertex3, scalene_vertex1)

scalene_triangle = ScalenTriangle([scalene_vertex1, scalene_vertex2, scalene_vertex3],[scalene_line1, scalene_line2, scalene_line3], [60, 60, 60])

square_vertex1 = Point(0, 0)
square_vertex2 = Point(0, 1)
square_vertex3 = Point(1, 1)
square_vertex4 = Point(1, 0)

square_line1 = Line(square_vertex1, square_vertex2)
square_line2 = Line(square_vertex2, square_vertex3)
square_line3 = Line(square_vertex3, square_vertex4)
square_line4 = Line(square_vertex4, square_vertex1)

square = Square([square_vertex1, square_vertex2, square_vertex3, square_vertex4],[square_line1, square_line2, square_line3, square_line4], [90, 90, 90, 90])

print("Información del triángulo equilátero:")
print("Perímetro:", equilateral_triangle.compute_perimeter())
print("Área:", equilateral_triangle.area_triangle())

print("\nInformación del triángulo isósceles:")
print("Perímetro:", isosceles_triangle.compute_perimeter())
print("Área:", isosceles_triangle.area_triangle())

print("\nInformación del triángulo escaleno:")
print("Perímetro:", scalene_triangle.compute_perimeter())
print("Área:", scalene_triangle.area_triangle())
print("Inner angles: ", scalene_triangle.compute_inner_angles())

print("\nInformación del cuadrado:")
print("Perímetro:", square.compute_perimeter())
print("Área:", square.compute_area())

if __name__ == "__main__":
  main()  
```

### Individual modules that import Shape in inheritates from it.

```css
estructura_archivos/
├── shapes2/
│   ├── main.py
│   ├── rectangle.py
│   ├── triangle.py
│   ├── shape2.py
```


```python
shape2.py

import math

class Point:
    def __init__(self, x: int, y: int):
        self._x = x
        self._y = y

    def get_x(self):
        return self._x

    def set_x(self, value):
        self._x = value

    def get_y(self):
        return self._y

    def set_y(self, value):
        self._y = value

    def compute_distance(self, other_point: 'Point'):
        return ((other_point.get_x() - self.get_x()) ** 2 + (other_point.get_y() - self.get_y()) ** 2) ** 0.5


class Line:
    def __init__(self, start_point: Point, end_point: Point):
        self.start_point = start_point
        self.end_point = end_point
    
    def length(self):
        calcu = self.start_point.compute_distance(self.end_point)
        return calcu

class Shape:
    is_Regular = True

    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        self.vertices = vertices
        self.edges = edges
        self.inner_angles = inner_angles

    def compute_area(self):
        pass
    
    def compute_perimeter(self):
        return sum(edge.length() for edge in self.edges)

    def compute_inner_angles(self):
        count = len(self.vertices)
        return (count-2) * 180

```

```python
rectangle.py

from shapes2 import shape2

class Rectangle(Shape):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 4:
            raise ValueError("A rectangle must have 4 vertices")

    def compute_area(self):
        length1 = self.edges[0].length()
        length2 = self.edges[1].length()
        return length1 * length2

class Square(Rectangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)       
        if len(vertices) != 4:
            raise ValueError("A Square must have 3 vertices")
        if self.edges[0].length() != self.edges[1].length():
            raise ValueError("A square must have equal sides")

```

```python

triangle.py

from shapes2 import shape2

class Triangle(Shape):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
    def area_triangle(self):
        s=0.5*self.compute_perimeter()
        return math.sqrt(s*(s-self.edges[0].length())*(s-self.edges[1].length())*(s-self.edges[2].length()))
    
class EquilateralTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not (self.edges[0].length() == self.edges[1].length() == self.edges[2].length()):
            raise ValueError("An Equilateral triangle must have three equal sides")
        
class IsoscelesTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not ((self.edges[0].length() == self.edges[1].length() != self.edges[2].length()) or
                (self.edges[0].length() == self.edges[2].length() != self.edges[1].length()) or
                (self.edges[1].length() == self.edges[2].length() != self.edges[0].length())):
            raise ValueError("An Isosceles triangle must have two equal sides")
        
class ScalenTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not ((self.edges[0].length() != self.edges[1].length() != self.edges[2].length())):
            raise ValueError("A Scalene triangle must have three different sides")

```
```python
main.py


from shapes2.shape2 import Line, Point
from shapes2.rectangle import Rectangle
from shapes2.triangle import Triangle


vertex1 = Point(0, 0)
vertex2 = Point(0, 1)
vertex3 = Point(1, 1)
vertex4 = Point(1, 0)

line1 = Line(vertex1, vertex2)
line2 = Line(vertex2, vertex3)
line3 = Line(vertex3, vertex4)
line4 = Line(vertex4, vertex1)

rectangle = Square([vertex1, vertex2, vertex3, vertex4], [line1, line2, line3, line4], [90, 90, 90, 90])

print("Perímetro del rectángulo:", rectangle.compute_perimeter())

print("Área del rectángulo:", rectangle.compute_area())
print(rectangle.compute_inner_angles())

triangle_vertex1 = Point(0, 0)
triangle_vertex2 = Point(3, 0)
triangle_vertex3 = Point(1.5, 3 * math.sqrt(3) / 2)  

triangle_line1 = Line(triangle_vertex1, triangle_vertex2)
triangle_line2 = Line(triangle_vertex2, triangle_vertex3)
triangle_line3 = Line(triangle_vertex3, triangle_vertex1)

equilateral_triangle = EquilateralTriangle([triangle_vertex1, triangle_vertex2, triangle_vertex3],[triangle_line1, triangle_line2, triangle_line3], [60, 60, 60])

isosceles_vertex1 = Point(0, 0)
isosceles_vertex2 = Point(3, 0)
isosceles_vertex3 = Point(1.5, 2)  

isosceles_line1 = Line(isosceles_vertex1, isosceles_vertex2)
isosceles_line2 = Line(isosceles_vertex2, isosceles_vertex3)
isosceles_line3 = Line(isosceles_vertex3, isosceles_vertex1)

isosceles_triangle = IsoscelesTriangle([isosceles_vertex1, isosceles_vertex2, isosceles_vertex3], [isosceles_line1, isosceles_line2, isosceles_line3], [60, 60, 60])

scalene_vertex1 = Point(0, 0)
scalene_vertex2 = Point(3, 0)
scalene_vertex3 = Point(2, 4)  

scalene_line1 = Line(scalene_vertex1, scalene_vertex2)
scalene_line2 = Line(scalene_vertex2, scalene_vertex3)
scalene_line3 = Line(scalene_vertex3, scalene_vertex1)

scalene_triangle = ScalenTriangle([scalene_vertex1, scalene_vertex2, scalene_vertex3],[scalene_line1, scalene_line2, scalene_line3], [60, 60, 60])

square_vertex1 = Point(0, 0)
square_vertex2 = Point(0, 1)
square_vertex3 = Point(1, 1)
square_vertex4 = Point(1, 0)

square_line1 = Line(square_vertex1, square_vertex2)
square_line2 = Line(square_vertex2, square_vertex3)
square_line3 = Line(square_vertex3, square_vertex4)
square_line4 = Line(square_vertex4, square_vertex1)

square = Square([square_vertex1, square_vertex2, square_vertex3, square_vertex4],[square_line1, square_line2, square_line3, square_line4], [90, 90, 90, 90])

print("Información del triángulo equilátero:")
print("Perímetro:", equilateral_triangle.compute_perimeter())
print("Área:", equilateral_triangle.area_triangle())

print("\nInformación del triángulo isósceles:")
print("Perímetro:", isosceles_triangle.compute_perimeter())
print("Área:", isosceles_triangle.area_triangle())

print("\nInformación del triángulo escaleno:")
print("Perímetro:", scalene_triangle.compute_perimeter())
print("Área:", scalene_triangle.area_triangle())
print("Inner angles: ", scalene_triangle.compute_inner_angles())

print("\nInformación del cuadrado:")
print("Perímetro:", square.compute_perimeter())
print("Área:", square.compute_area())


if __name__ == "__main__":
    main()

```
