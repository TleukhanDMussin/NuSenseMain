
# NUSense: Robust Soft Optical Tactile Sensor

This repository contains the code and design files for **NUSense**, a robust optical tactile sensor designed for detecting shear strain through image processing. The project was developed by researchers at Nazarbayev University and National University of Singapore and submitted to **ICRA 2025**.

## Project Overview

**NUSense** is an optical tactile sensor that detects **shear strain** using image analysis. The sensor is built using a soft silicone pad, dyed with colored inks, which deforms under mechanical loads. A camera beneath the sensor captures the deformation patterns, which are processed to estimate force and contact properties. The sensor’s design and image-processing algorithms enable real-time force estimation, contact localization, and edge detection.

### Key Features
- **Shear Strain Detection**: Captures deformation patterns using a soft silicone pad and an embedded camera.
- **Optical Sensing**: Uses color-based optical feedback to detect surface strain.
- **Robust Design**: Designed for repetitive tasks and able to withstand varying contact conditions, including damage to the outer layer.
- **Versatile Applications**: Useful for robotic manipulation, force sensing, and contact localization.

## The Display of a touch from the sensor camera
<img src="tests/x2_z1_Cut.gif" alt="Sample Output 1" width="500" height="auto">

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
  - [Code Structure](#code-structure)
  - [Running the Sensor Processing Code](#running-the-sensor-processing-code)
- [Experimental Results](#experimental-results)
- [Contributing](#contributing)
- [License](#license)

## Installation

### Requirements
- Python 3.x
- OpenCV
- NumPy
- Matplotlib
- GeomDL (B-Spline library)

### Install Dependencies
Clone this repository and navigate to the project directory:
```bash
git clone https://github.com/your-username/NUSense.git
cd NUSense
```

Install the dependencies using pip:
```bash
pip install -r requirements.txt
```

The required libraries will be installed, including OpenCV for image processing, Matplotlib for visualization, and GeomDL for B-Spline surface processing.

## Usage

### Code Structure
The repository is organized as follows:
```
NUSense/
│
├── data/                # Contains sample images for testing
├── src/                 # Main codebase for image processing
│   ├── nusense.py       # Core class for image processing
│   ├── defisheye.py     # Fisheye correction function
│   ├── utils.py         # Utility functions
│   └── ...
│
├── docs/                # Documentation files
├── tests/               # Unit tests for the image processing pipeline
├── results/             # Output of experiments, including plots and results
│
└── README.md            # Project README file
```

### Running the Sensor Processing Code
1. **Prepare the Images**:
   - Place the input fisheye images in the `data/` directory.
   - Ensure that the images follow the correct naming convention for ease of processing.

2. **Run the Main Script**:
   Use the following command to process the fisheye images and extract shear strain:
   ```bash
   python src/nusense.py
   ```

   The script will perform the following steps:
   - Apply **fisheye correction** to raw images using camera matrix calibration.
   - Extract **control points** from the deformation patterns using B-Spline fitting.
   - Calculate **shear strain** and display results as visual plots.

3. **Output**:
   - The processed images and plots of control points, B-Spline surfaces, and shear strain metrics will be saved in the `results/` directory.
   - Visualizations include 2D and 3D plots of deformation, as well as contact localization and edge detection results.

### Example Code Usage
Here is an example of how to use the `NUSense` class to process an input image and calculate shear strain:

```python
from src.nusense import NUSense

# Load an image
img_path = 'data/experiment1/8N/x1_z1.jpg'
img = cv2.imread(img_path)

# Initialize NUSense class
nusense = NUSense()

# Apply fisheye correction
img_undistorted = nusense.defisheye(img)

# Apply Sobel and bilateral filtering
roi_x, roi_y, roi_w, roi_h = 390, 203, 1335, 1000
img_roi = nusense.apply_sobel_with_bilateral_filter_to_image(
    img_undistorted, roi_x, roi_y, roi_w, roi_h)

# Calculate shear strain
P_diff, P1, P2 = nusense.calculate_euclidean(img_roi, img_roi)

# Visualize results
nusense.visualize_euclidean_distances(P1, P2, P_diff)
```

## Experimental Results

The NUSense sensor was tested using a robotic arm, performing the following experiments:
1. **Normal Force Estimation**: Estimation of normal forces ranging from 1N to 8N using round and flat indenters.
2. **Single Contact Localization**: Accurate localization of single contacts on the sensor surface.
3. **Hysteresis Analysis**: Measurement of force during loading and unloading cycles, demonstrating the viscoelastic behavior of the sensor.
4. **Robustness Testing**: Consistency of shear strain estimation over 70 repetitive cycles.
5. **Edge Detection**: Detection of edge orientation using PCA, with clear shear strain patterns.
6. **Performance with Damaged Sensor**: The sensor retains its functionality even with a torn outer layer, highlighting its robustness in adverse conditions.

### Visual Results
![Sample Output 1](results/euclidean_distances.png)
*Figure: Visualization of Euclidean distances calculated from deformation patterns.*

![Sample Output 2](results/control_points_image1.png)
*Figure: 2D scatter plot of control points for Image 1.*


## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
