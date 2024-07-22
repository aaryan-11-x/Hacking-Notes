```sh
pip install pygame
pip install PyOpenGL PyOpenGL_accelerate
```

```python
import pygame as pg
from OpenGL.GL import *

class App:
	# Initialize Pygame
    def __init__(self) -> None:
        pg.init()
        pg.display.set_mode((640, 480), pg.OPENGL | pg.DOUBLEBUF)
        self.clock = pg.time.Clock()
	# Initialize OpenGL
        glClearColor(0.1, 0.2, 0.2, 1)
        self.main_loop()
```

##### Importing Libraries
`from OpenGL.GL import *`: This line imports all the functions and constants from the OpenGL.GL module, allowing direct access to them without having to use the module name.


##### Constructor Functions
-   `pg.init()`: Initializes the Pygame library, preparing it for use.
-   `pg.display.set_mode((640, 480), pg.OPENGL | pg.DOUBLEBUF)`: Creates a window with dimensions 640x480 and sets the window flags to `pg.OPENGL` and `pg.DOUBLEBUF`. This means the window will use OpenGL for rendering and will be double-buffered for smoother graphics.
	- [ ] **DOUBLEBUF** : Systen where 2 drawable surfaces are used, One frame is being drawn to by the CPU/GPU while the other is visible on the screen.

-   `self.clock = pg.time.Clock()`: Creates a clock object used to control the frame rate of the application.


##### OpenGL functions
`glClearColor(0.1, 0.2, 0.2, 1)`: Sets the clear color for the OpenGL rendering context. The arguments (0.1, 0.2, 0.2, 1) represent the red, green, blue, and alpha components of the color, respectively. In this case, the clear color is set to a slightly **bluish-green** color.

```python
# ...Inside App Class
def main_loop(self):
        while True:
            for event in pg.event.get():
                if event.type == pg.QUIT:
                    break
            # Refresh Screen
            glClear(GL_COLOR_BUFFER_BIT)
            pg.display.flip()

            # Timing
            self.clock.tick(60)

def quit(self):
        pg.quit()
```

This method implements the main loop of the application, where events are handled, the screen is refreshed, and timing is managed.
1.  `for event in pg.event.get():`: This loop iterates over all the events that have occurred since the last time this line of code was executed. Events are user inputs, such as keyboard presses, mouse clicks, or window-related actions.
2.  `if event.type == pg.QUIT:`: This condition checks if the current event is a "QUIT" event, which occurs when the user closes the application window. If such an event is detected, the loop is broken, and the program will proceed to exit the while loop.
3.   `glClear(GL_COLOR_BUFFER_BIT)`: Clears the color buffer of the OpenGL context, effectively wiping the screen with the previously set clear color.
4.   `pg.display.flip()`: Swaps the back and front buffers to display the rendered frame.
5.   `self.clock.tick(60)`: Limits the frame rate to 60 frames per second using the Pygame Clock object. This ensures the program doesn't run too fast and uses a controlled frame rate.


#### Basic Code
```python
from OpenGL import *
from OpenGL.GL import *
from OpenGL.GLUT import *
from OpenGL.GLU import *
def showScreen():
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT) # Remove everything from screen (i.e. displays all white)
	glutInit() # Initialize a glut instance which will allow us to customize our window
	glutInitDisplayMode(GLUT_RGBA) # Set the display mode to be colored
	glutInitWindowSize(500, 500) # Set the width and height of your window
	glutInitWindowPosition(0, 0) # Set the position at which this windows should appear
	wind = glutCreateWindow("OpenGL Coding Practice") # Give your window a title
	glutDisplayFunc(showScreen) # Tell OpenGL to call the showScreen method continuously
	glutIdleFunc(showScreen) # Draw any graphics or shapes in the showScreen function at all times
	glutMainLoop() # Keeps the window created above displaying/running in a loop
```
