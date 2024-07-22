```python
import pygame as pg
from OpenGL.GL import *
import numpy as np
  
class App:
    # Copy from Setup

  
class Triangle:
    def __init__(self) -> None:
        # x, y, z, r, g, b (Range is from -1 to 1)
        self.vertices = ((-0.5, -0.5, 0, 1.0, 0.0, 0.0),
                         (0.5, -0.5, 0.0, 0.0, 1.0, 0.0), (0.0, 0.5, 0, 0, 0, 1.0))

        self.vertices = np.array(self.vertices, dtype=np.float32)
        self.vertices_count = 3
        self.vao = glGenVertexArrays(1)
        glBindVertexArray(self.vao)
        self.vbo = glGenBuffers(1)
        glBindBuffer(GL_ARRAY_BUFFER, self.vbo)
        glBufferData(GL_ARRAY_BUFFER, self.vertices.nbytes, self.vertices, GL_STATIC_DRAW)
        glEnableVertexAttribArray(0)    # Attribute 0 is Position
        glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 24, ctypes.c_void_p(0))
        glEnableVertexAttribArray(1)    # Attribute 1 is Colour
        glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 24, ctypes.c_void_p(12))

  
if __name__ == "__main__":
    myapp = App()
```


1.  `self.vertices`: This line initializes the "vertices" variable. It's a tuple containing three sets of values, each representing a vertex of the triangle. Each vertex is defined in 3D space, and the last three values (r, g, b) represent the color of the vertex in red, green, and blue components.
    
2.  `self.vertices = np.array(self.vertices, dtype=np.float32)`: This line converts the tuple of vertices into a NumPy array with a data type of float32. It allows us to handle the vertex data more efficiently in OpenGL. It should be set to 32-bit else OpenGL won't read the data properly.
    
3.  `self.vertices_count = 3`: This line sets the number of vertices in the triangle. Since we have three vertices, we set this variable to 3.
    
4.  `self.vao = glGenVertexArrays(1)`: This line generates a *Vertex Array Object* (VAO). The VAO is a container that holds configurations of vertex attributes, such as positions and colors.
    
5.  `glBindVertexArray(self.vao)`: This line binds (makes active) the VAO for subsequent operations. Any changes we make to vertex attribute configurations will be stored in this VAO. *Plain Vertex data without configurations is useless so we need attributes*.
    
6.  `self.vbo = glGenBuffers(1)`: This line generates a *Vertex Buffer Object* (VBO). Buffer is a Basic Storage Container. The VBO is used to store the actual vertex data (positions and colors).
    
7.  `glBindBuffer(GL_ARRAY_BUFFER, self.vbo)`: This line binds (makes active) the VBO as the current buffer for vertex array data. Subsequent buffer operations will target this VBO.
    
8.  `glBufferData(GL_ARRAY_BUFFER, self.vertices.nbytes, self.vertices, GL_STATIC_DRAW)`: This line allocates and fills the VBO with the vertex data from the NumPy array. It tells OpenGL how much data to allocate (self.vertices.nbytes) and provides the vertex data (self.vertices). The *GL_STATIC_DRAW* hint indicates that the data won't change frequently, optimizing memory usage.
    - `self.vertices.nbytes`: This is the size of the data in bytes that we want to store in the VBO. `self.vertices` is the NumPy array containing the vertex data, and `nbytes` is an attribute of NumPy arrays that *represents the size of the data in bytes*.

9.  `glEnableVertexAttribArray(0)`: This line enables the generic vertex attribute at index 0. In our case, index 0 represents the *position* of each vertex.

10.  `glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 24, ctypes.c_void_p(0))`: This line specifies the format of the vertex attribute at index 0. It tells OpenGL how to interpret the vertex data for positions. Here, we have 3 components (x, y, z), represented as GL_FLOAT data type, with no special considerations for normalization (GL_FALSE), and starting from the *beginning of the array (offset 0)*.
    - `GL_FALSE`: This parameter indicates whether the attribute data should be normalized or not. When it's `GL_TRUE`, the *attribute data will be normalized between 0.0 and 1.0* (e.g., for unsigned integer values, they will be divided by the maximum value). When it's `GL_FALSE`, the attribute data will be used as-is without normalization.
    - `24` is the offset, Max. 32-bit number, so 1 Number has 4 bytes. There are 6 numbers for each vertex, so 4\*6 = 24 bytes

11.  `glEnableVertexAttribArray(1)`: This line enables the generic vertex attribute at index 1. In our case, index 1 represents the *color* of each vertex.

12.  `glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 24, ctypes.c_void_p(12))`: This line specifies the format of the vertex attribute at index 1. It tells OpenGL how to interpret the vertex data for colors. Here, we have 3 components (r, g, b), represented as GL_FLOAT data type, with no special considerations for normalization (GL_FALSE), and starting from an offset of 12 bytes in the array, effectively skipping the (x, y, z) components.
	- `12` as the first colour starts after 3 numbers into the buffer(x,y,z, r...). So 3\*4 = 12 bytes.
	- `ctypes.c_void_p(12)` : `ctypes` is a Python library used for working with C data types, and `c_void_p` is a C data type representing a *pointer to a memory address*. It is used as an *offset to indicate where the color data starts* in the `self.vertices` array. Since the color components (r, g, b) come after the position components (x, y, z), and each component is represented as a float32 (4 bytes each), we skip the first 12 bytes (3 components * 4 bytes each) to reach the color data.



#### Simplified Explanation
1.  Import the necessary libraries for working with graphics in OpenGL.
2.  Create a class called "Triangle" to represent a triangle.
3.  Inside the class constructor (`__init__`), define the vertices of the triangle and their corresponding colors.
4.  Convert the vertices data into a more efficient format using NumPy.
5.  Generate a Vertex Array Object (VAO) to store the vertex configurations.
6.  Bind the VAO to make it active for future operations.
7.  Generate a Vertex Buffer Object (VBO) to store the actual vertex data.
8.  Bind the VBO to make it active for subsequent buffer operations.
9.  Fill the VBO with the vertex data.
10.  Enable the vertex attributes for positions and colors.
11.  Define how OpenGL should interpret the vertex data for positions and colors.