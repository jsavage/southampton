DRAFT / Prototype - Interactive Tidal Curve Visualization Widget
Overview
A web-based (Jupyter or standalone browser) interactive visualization tool for working with tidal curves, combining:

An annotated tidal curve image,

Editable and computed time boxes,

Draggable "Low Water" (LW) and "High Water" (HW) scale markers,

Live-drawn lines including automatic color-based intersection detection with the underlying tidal curve.

Functional Requirements
1. Background Image and Calibration
Render a fixed-size (1100x570 px) canvas showing a tidal curve background image (JPEG/PNG, delivered as base64).

Horizontal axes and calibration for time/data overlays are based on provided pixel coordinates for scales and regions of interest.

2. Time Input Boxes
Overlay 14 input boxes horizontally at the calibrated rectangle: left at x=389px, right at x=1054px, vertical between y=516px and y=543px.

Names and order (left to right): PreHW, LW-5, LW-4, LW-3, LW-2, LW-1, LW, LW+1, LW+2, LW+3, LW+4, LW+5, LW+6, PostHW.

Only PreHW, LW, and PostHW are fully editable; the rest are readonly and display calculated values.

Boxes use a consistent condensed font/size and do not display a native browser time picker.

Each box is individually addressable by ID.

3. Interactive Scale Markers (Draggable Dots)
Show two draggable red dots overlaying the tidal curve:

LW Dot: moves horizontally along a fixed y on the "LW" scale (x between 78.5 and 312, y=503.3).

HW Dot: moves horizontally along a fixed y on the "HW" scale (x between 72 and 466, y=105.3).

Dots are only movable on their own respective horizontal scale.

While dragging, display the current dot coordinates in a live-updating label below the canvas.

4. Connecting and Annotative Lines
Draw a purple line connecting the current LW and HW dots.

Draw a vertical yellow line at the center x of the LW input box. The starting y value for this yellow line is chosen by:

If the user is not dragging a scale, the vertical yellow line starts at the current mouse y position when clicking or moving the mouse.

If the user is dragging one of the scale dots, the yellow line starts at the bottom (y=543) of the LW box.

The yellow line proceeds upward until it intersects the "red" tidal curve in the image.

Red curve intersection is detected by inspecting canvas pixel data for a suitably "red" pixel (r > 150, g/b < 80).

From the intersection point with the red curve:

Draw a horizontal leftwards yellow line until it meets the purple LW-HW line (solve for intersection).

From this intersection, draw a vertical upward yellow line, ending at the intersection with the purple line.

Place a yellow arrowhead at the end of this final vertical yellow line, pointing towards the HW scale.

All intersections are computed with pixel scanning and basic geometry.

5. Mouse Interaction
Update the yellow line’s vertical starting position dynamically in response to mouse movement/clicks except when dragging dots.

The app always reflects the latest user interactions (mouse move, click, dragging a dot).

6. General UI/JS Integration
The entire system runs in a browser or Jupyter cell via an HTML/JS block (IPython display or static HTML).

All dynamic overlays (dots, lines, arrows) are redrawn on each user interaction, using canvas APIs.

Inputs and mouse handlers are dynamically attached and removed when necessary.

Some attempt to have: No global state leaks: each instance must manage its own events and DOM elements, making it safe for reruns in a Jupyter environment.
- Not sure if this is achieved
- 
Technical Notes
All pixel positions for overlays and scales are calibrated to the specific background image.

The solution should be presented as a single, self-contained HTML/JS (or HTML/JS/Python in Jupyter) block for reproducibility.

The code must be as readable and modular as possible to allow further extension (ex: outputting calibration results, supporting new backgrounds).
