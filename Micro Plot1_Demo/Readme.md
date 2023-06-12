# Random Scatter Plot Generator

This Python script generates a random scatter plot and serves it as an SVG image using a web server. The Microdot framework is used for web development, and the scatter plot is created using the matplotlib library.

## Prerequisites

- Python 3.x
- matplotlib
- microdot

## How to Run

1. Install the required dependencies by running the following command:
   ```
   pip install matplotlib microdot
   ```

2. Run the script by executing the following command:
   ```
   python scatter_plot_generator.py
   ```

3. Open a web browser and visit `http://localhost:8008` to see the random scatter plot.

## Code Explanation

The code generates a random scatter plot using matplotlib and serves it as an SVG image through a web server.

```python
import base64
import io
from io import BytesIO
import numpy as np
from microdot import Microdot, Response
from matplotlib.figure import Figure
from matplotlib.backends.backend_agg import FigureCanvasAgg as FigureCanvas

app = Microdot()
```

The script imports the necessary modules: `base64`, `io`, `numpy`, `Microdot`, `Response`, `Figure`, and `FigureCanvas`. It also creates an instance of the `Microdot` application.

```python
@app.route('/')
@app.route('/<points>')
def hello(request, points="10"):
    points = int(points)

    data = np.random.rand(points, 2)

    fig = Figure()
    FigureCanvas(fig)

    ax = fig.add_subplot(111)

    ax.scatter(data[:, 0], data[:, 1])

    ax.set_xlabel('x')
    ax.set_ylabel('y')
    ax.set_title(f'There are {points} data points!')
    ax.grid(True)

    img = io.StringIO()
    fig.savefig(img, format='svg')
    # clip off the xml headers from the image
    svg_img = '<svg' + img.getvalue().split('<svg')[1]

    return Response(svg_img, headers={'Content-Type': 'image/svg+xml'})
```

The script defines two URL routes. The root route (`/`) and an optional route parameter (`/<points>`) are used to handle incoming requests. The `hello()` function generates a random scatter plot based on the provided number of points. It creates a `Figure` and `FigureCanvas` from matplotlib. Then, it creates a scatter plot using the randomly generated data points. The resulting plot is saved as an SVG image.

```python
app.run(host="0.0.0.0", port=8008, debug=True)
```

The script runs the Microdot application, enabling the web server to listen on port 8008 and handle incoming requests.

**Note:** Remember to update the port number and other details if necessary when running the application.
