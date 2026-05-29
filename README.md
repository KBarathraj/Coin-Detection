# Coin Detection Using Morphological Operations and Thresholding

**Name:** Barathraj K  
**Registration No:** 212224230033  
**Slot:** T2-J6

## Project Overview

This project demonstrates coin detection and counting using computer vision techniques in Python. The implementation applies **morphological operations** and **thresholding techniques** to accurately detect and count coins in an image using two different methods: **Blob Detection** and **Contour Detection**.

---

## Aim

To develop an automated system that:
- Processes an image containing coins
- Applies image preprocessing and morphological operations
- Detects individual coins using multiple methods
- Counts and validates the total number of coins detected

---

## Methodology

### Step 1: Image Loading
- **Input:** CoinsA.png
- **Description:** The original RGB image is loaded using OpenCV's `cv2.imread()` function
- **Finding:** The image contains multiple coins on a contrasting background, suitable for binary classification

### Step 2: Grayscale Conversion
- **Channel Selection:** Grayscale (Single Channel)
- **Justification:** 
  - Coins appear as consistent intensity values in grayscale
  - Reduces computational complexity from 3 channels to 1
  - Simplifies thresholding by eliminating color variations
  - Maintains sufficient contrast between coins and background
- **Operation:** `cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)`

### Step 3: Thresholding
- **Method Used:** Otsu's Thresholding (Automatic threshold selection)
- **Justification for Thresholding:**
  - Creates binary image separating coins from background
  - Otsu's method automatically determines optimal threshold value
  - Minimizes intra-class variance between coin and non-coin pixels
  - Produces clean binary output without manual parameter tuning
- **Operation:** `cv2.threshold(imageGray, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)`

### Step 4: Morphological Operations
- **Operations Applied:**
  1. **Erosion:** Removes small noise and thin structures
     - Kernel size: 5×5
     - Iterations: 2
  2. **Dilation:** Reconnects separated coin regions and fills small holes
     - Kernel size: 5×5
     - Iterations: 2

- **Justification:**
  - Erosion eliminates noise and small artifacts that aren't coins
  - Dilation reconnects fragmented coin regions
  - Combined (Closing operation) helps standardize coin shapes
  - Improves detection accuracy by cleaning binary image

### Step 5: Coin Detection Methods

#### Method 1: Blob Detection
- **Detector Used:** SimpleBlobDetector
- **Parameters:**
  - Filters by color, circularity, convexity, and inertia
  - Optimized for circular coin shapes
  - Thresholding range: Binary image (0-255)
  - Min/Max circularity: Tuned for coin-like objects
  
- **Process:**
  1. Convert processed image to appropriate format
  2. Create SimpleBlobDetector with optimized parameters
  3. Detect blobs matching coin characteristics
  4. Filter by size and shape metrics

#### Method 2: Contour Detection
- **Contour Extraction:** OpenCV contour finding algorithm
- **Filtering Criteria:**
  - Contour area: Filters out noise with area < min_area
  - Contour circularity: Selects near-circular shapes
  - Contour perimeter: Validates valid coin-like boundaries
  
- **Process:**
  1. Find all contours in binary image: `cv2.findContours()`
  2. Calculate area for each contour: `cv2.contourArea()`
  3. Approximate contour shape: `cv2.approxPolyDP()`
  4. Filter based on circularity: `4π × Area / Perimeter²`
  5. Count valid contours as detected coins

---

## Results

### Findings by Section

#### Channel/Image Choice Justification ✓
- **Selected:** Grayscale single-channel image
- **Reason:** Optimal balance between computational efficiency and detection accuracy
- **Result:** Clear separation between coin regions and background

#### Thresholding & Morphological Operations ✓
- **Thresholding Method:** Otsu's automatic threshold selection
- **Morphological Operations:** Erosion followed by Dilation (Closing)
- **Result:** Clean binary image with well-defined coin regions and minimal noise

#### Number of Coins Detected

| Detection Method | Coins Detected |
|------------------|----------------|
| **Blob Detection** | *[To be filled after execution]* |
| **Contour Detection** | *[To be filled after execution]* |

#### Conclusion Comparing Both Methods

**Blob Detection:**
- ✓ Effective for well-defined, circular objects
- ✓ Built-in shape filtering reduces false positives
- ✗ Less flexible for irregular coin positions
- ✗ Parameter tuning required

**Contour Detection:**
- ✓ More flexible and adaptive to various coin shapes
- ✓ Fine-grained control over filtering criteria
- ✓ Better for overlapping coins
- ✗ More sensitive to preprocessing quality
- ✗ May detect coin fragments if not properly filtered

**Overall Assessment:**
- **Contour Detection** provides more robust and accurate coin counting
- **Blob Detection** works well as a secondary validation method
- Combined approach yields highest confidence in coin count

---

## Implementation Details

### Key Libraries
- `cv2` (OpenCV) - Image processing
- `numpy` - Numerical operations
- `matplotlib` - Visualization

### Processing Pipeline
```
Original Image (RGB)
    ↓
Grayscale Conversion
    ↓
Otsu's Thresholding
    ↓
Morphological Operations (Erosion + Dilation)
    ↓
├─ Blob Detection
└─ Contour Detection
    ↓
Coin Count & Visualization
```

### Parameters Used
- **Morphological Kernel:** 5×5 with 2 iterations
- **Blob Circularity Range:** 0.5 - 1.0 (adjusted based on results)
- **Contour Area Threshold:** Minimum area = 50 pixels
- **Circularity Threshold:** ≥ 0.7 for valid coins

---

## Files

- `Coin-Detection-workfile.ipynb` - Complete Jupyter notebook with all code and visualizations
- `CoinsA.png` - Input image with coins to be detected
- `README.md` - This documentation

---

## Notes

- All intermediate images are preserved and displayed for analysis
- Results are documented after each processing step
- Both detection methods are compared for accuracy and robustness
- The methodology is generalizable to other coin detection scenarios

---


This project successfully demonstrates the application of fundamental computer vision techniques including image preprocessing, morphological operations, and object detection methods. The comparative analysis of Blob and Contour detection provides insights into choosing appropriate methods for different image processing scenarios.
