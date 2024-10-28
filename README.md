Closest Ball Detection
==============================

This Jupyter Notebook project is designed for an **FRC (FIRST Robotics Competition)** application where the goal is to detect and locate the closest ball to the camera based on color and contour analysis. The notebook utilizes computer vision techniques with OpenCV to identify balls based on color and shape, then calculates each ball's approximate distance and angle relative to the camera to highlight the nearest one.

![download](https://github.com/user-attachments/assets/0f2bb70f-5c34-4d0f-b57a-006c9ccb491d)

Requirements
------------

-   **Python 3**
-   **Jupyter Notebook**
-   **OpenCV** (cv2)
-   **NumPy**
-   **Matplotlib**
-   **Imutils**

To install dependencies:

`pip install opencv-python numpy matplotlib imutils`

Key Features
------------

-   **Ball Detection**: Uses HSV color filtering to isolate balls based on color range.
-   **Shape Detection**: Identifies circular contours to isolate balls from other objects.
-   **Distance Calculation**: Approximates each ball's distance from the camera based on its perceived width.
-   **Angle Calculation**: Calculates the angle of the ball from the camera's centerline.
-   **Display of Results**: Annotates the detected image with distance, angle, and closest ball highlighted.

Workflow Explanation
--------------------

1.  **HSV Filtering**: Converts the input image to HSV color space and applies a mask based on a defined HSV range to isolate balls of a specific color.
2.  **Contour Detection**: Finds contours in the masked image, then filters them based on shape (using the ShapeDetector class) to retain only circular objects.
3.  **Distance and Angle Calculation**:
    -   Uses contour width to approximate the distance of each detected ball based on a calibrated range.
    -   Calculates the angle from the center of the image to the ball's centroid, giving an estimate of its position relative to the robot.
4.  **Highlighting Closest Ball**: Identifies the closest ball, annotates its distance and angle, and displays the final image with the ball highlighted.

Key Functions and Classes
-------------------------

### `ShapeDetector`

Detects basic shapes within contours. Identifies shapes based on the number of vertices, distinguishing circles for ball detection.

### `find_min_max_xcoord`

Calculates the minimum and maximum x-coordinates of an object's contour to measure the object's width.

### `interpolate_distance_from_object`

Estimates distance based on the object's pixel width by interpolating between predefined distances.

### `angle_from_center`

Computes the angle of a point from the camera's perspective relative to the bottom-center of the image.

### `find_closest_distance`

This is the main function that:

-   Masks the image to identify objects within the specified HSV range.
-   Detects circular objects with sufficient area.
-   Computes each detected object's distance and angle, identifies the closest ball, and highlights it with annotations.

Usage
-----

1.  **Set HSV Range**: Define the lower and upper HSV bounds for the target ball color (e.g., `yellow_lower` and `yellow_upper` for yellow balls).
2.  **Adjust Minimum Area Size**: Set the `min_area_size` to filter out noise and capture objects of sufficient size.
3.  **Load Image**: Use `mpimg.imread` to load an image with visible balls.
4.  **Run Detection**: Call `find_closest_distance` with the image and other parameters.
5.  **Display Results**: Use `matplotlib.pyplot` to display the annotated image showing the detected balls and highlighting the closest one.

Sample Code
-----------

```python
# Define color range for yellow
yellow_lower = np.array([90, 110, 70])
yellow_upper = np.array([100, 255, 255])
minimum_area_size = 4500

# Load image and find closest ball
image = mpimg.imread('woodfloor_3balls.jpg')
ball_img = find_closest_distance(image, yellow_lower, yellow_upper, minimum_area_size, 31, True, True)

# Display result
plt.figure(figsize=(12, 12))
plt.imshow(ball_img)
plt.show()
```

Notes and Calibration
---------------------

-   **Camera Calibration**: Adjust `interpolate_distance_from_object` parameters (`_MAX_WIDTH` and `_MIN_WIDTH`) based on the specific camera setup and environment.
-   **Color Range Tuning**: Ensure the HSV range matches the actual color of the balls in your images.
-   **Image Resolution**: Higher resolution may yield more accurate results but could impact performance.

Future Enhancements
-------------------

-   Integrate real-time video stream detection.
-   Add further object filtering to reduce false positives.
-   Adapt to varying light conditions by dynamically adjusting the HSV range.

This notebook provides a solid base for FRC teams to implement robust ball detection, improving accuracy in object tracking and distance estimation for better game performance.
