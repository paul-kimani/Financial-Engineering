### 1. The Grayscale Image (A 2D Matrix)

Imagine a simple black-and-white photograph. If you zoom all the way in, the image is just a grid of pixels (Height $\times$ Width).

To a computer, this grid is just a **2D matrix**. Each cell in the matrix holds a single number (a scalar) representing brightness, usually ranging from 0 (pure black) to 255 (pure white).

- If your image is 1080 pixels high and 1920 pixels wide, it is stored in memory as a $1080 \times 1920$ matrix.

### 2. The Colour Image (A 3D Tensor)

Colour screens don't just use one shade of light; they create every color you see by mixing **R**ed, **G**reen, and **B**lue light.

This means a single number is no longer enough to represent a pixel. Every single pixel now needs **three** numbers (a 1D vector) to tell the screen how much Red, Green, and Blue to emit.

Because every pixel now holds three values, a flat 2D matrix is no longer sufficient to store the data. We have to add a third dimension, often called "depth" or "channels".

### 3. Visualizing the "Stack"

The easiest way to visualize this 3D tensor is to imagine peeling the colors apart into three separate, transparent layers:

1. **The Red Matrix:** A 2D grid storing _only_ the red intensity values (0-255) for every pixel.
2. **The Green Matrix:** A 2D grid storing _only_ the green intensity values.
3. **The Blue Matrix:** A 2D grid storing _only_ the blue intensity values.

When you stack these three 2D matrices on top of each other, you get a **3D Tensor**.

In Python libraries like PyTorch or NumPy, if you load that same $1080 \times 1920$ image, the software will report the tensor's shape as `(3, 1080, 1920)` or `(1080, 1920, 3)` depending on the framework. The `3` represents the RGB channels.
