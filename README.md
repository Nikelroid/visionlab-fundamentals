
# VisionLab: Fundamental Image Processing Algorithms

![Python](https://img.shields.io/badge/python-3.8%2B-blue?logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/numpy-1.24%2B-013243?logo=numpy&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-4.5%2B-red?logo=opencv&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Data_Viz-orange?logo=matplotlib&logoColor=white)


## Description
This repository contains a collection of foundational computer vision scripts implemented in Python using OpenCV and NumPy. The project focuses on understanding the mathematical underpinnings of image manipulation by implementing algorithms from scratch—or comparing manual implementations against library functions—rather than solely relying on high-level APIs.

Key areas covered include color channel alignment for historical photography, histogram specification for tone mapping, and optimized convolution techniques for filtering.


## Features
* **Prokudin-Gorskii Alignment:** Reconstructs color images from historical glass plate negatives using a multi-scale image pyramid search for optimal channel alignment ($R, G, B$).
* **Exact Histogram Matching:** An algorithm to map the pixel intensity distribution of a source image to match a target reference image exactly.
* **Selective Color Processing:** Isolates specific hues (e.g., pink flowers) for manipulation while applying effects (blur) to the background.
* **Vectorized Convolution:** Implements a custom box blur using array slicing and shifting to compare performance against iterative loops and OpenCV's optimized C++ backend.
* **Adaptive Contrast Enhancement:** Enhances image brightness and contrast while preventing over-saturation in high-luminance areas (e.g., skies).

## File Descriptions & Implementation Details

### 1. `Enhance1-1/q1.py` - Basic Enhancement
Enhances a dark, low-contrast image.
* **Method:** Converts the image to HSV space and applies a non-linear transformation to the Value (V) channel: $V_{new} = (\frac{V}{256})^{0.3} \times 1.9$.
* **Goal:** To increase visibility in shadow regions.

### 2. `Enhance2-2/q2.py` - Adaptive Enhancement
Improves upon `q1.py` by adding conditional logic to the enhancement curve.
* **Method:** Checks if the enhanced value deviates significantly from the original in bright regions. If the deviation is too high, the original value is preserved to prevent "washout" in the sky or bright light sources.

### 3. `PictureMatching-3/q3.py` - Channel Alignment
Aligns the three separate grayscale channels of Prokudin-Gorskii glass plates to create a color image.
* **Algorithm:** Uses an **Image Pyramid** approach. It downscales the image recursively to find a rough alignment (displacement vector) on smaller versions, then refines the alignment on larger versions.
* **Metric:** Sum of Absolute Differences (SAD) is used to measure similarity between the Green/Blue and Red/Blue channels.
* **Post-Processing:** Includes an auto-cropping feature using Canny edge detection to remove the uneven borders of the photographic plates.


### 4. `Flowers-4/q4.py` - Selective Hue & Blur
Creates a "portrait mode" or "color splash" effect based on specific color ranges.
* **Logic:** Iterates through the image in HSV space.
    * **Detection:** Checks if the Hue falls within the pink/purple range (135-175).
    * **Action:** If detected, shifts the Hue (color change). If not, replaces the pixel's Value/Saturation with the average of its $5 \times 5$ neighborhood (Box Blur).

### 5. `Pink-5/q5.py` - Blur Benchmarking
Benchmarks three different methods of implementing a Box Blur filter.
1.  **OpenCV:** `cv2.blur` (Highly optimized C++).
2.  **Iterative:** Nested `for` loops calculating averages pixel-by-pixel (Slow).
3.  **Vectorized (Manual):** Uses NumPy array slicing and `np.delete` to create shifted copies of the image matrix and sums them up, avoiding slow Python loops.

### 6. `Dark-6/q6.py` - Histogram Matching
Matches the histogram of a "Dark" source image to a "Pink" reference image.
* **Algorithm:** Calculates the difference between the source and target histograms and iteratively shifts pixel values in the source image until its cumulative distribution function (CDF) matches the target.
* **Visualization:** Plots the RGB histograms of the source, target, and result using Matplotlib.


## Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/nikelroid/image-peocessing-1.git](https://github.com/nikelroid/image-peocessing-1.git)
    cd image-peocessing-1
    ```

2.  **Install dependencies:**
    ```bash
    pip install numpy opencv-python matplotlib
    ```

## Usage

Navigate to the specific directory for the problem you want to run. Ensure the input images (e.g., `Dark.jpg`, `Pink.jpg`, `.tif` files) are present in the directory.

**Running Histogram Matching:**
```bash
cd Dark-6
python q6.py
````

**Running Prokudin-Gorskii Alignment:**

```bash
cd PictureMatching-3
python q3.py
```

*Note: This script expects the raw `.tif` or `.jpg` glass plate scan as input.*

**Running Blur Benchmark:**

```bash
cd Pink-5
python q5.py
```

## Contributing

Pull requests are welcome. Please ensure that any new algorithm implementations include a performance comparison or a theoretical explanation in the comments.

## License

This project is open-source and available under the **MIT License**.

## Contact

For questions regarding the algorithms, please open an issue in the repository.
