# 🛒 Retail Shelf Detection and ORB Feature Matching 🎯

This project demonstrates a classical computer vision pipeline for detecting retail shelf regions, extracting local features using ORB, matching features between images, and refining detections using Non-Maximum Suppression (NMS). The entire workflow is built with **OpenCV**, **NumPy**, and **Matplotlib**. 🖼️

## 🎯 Project Objective

The main objective is to process a retail store image to:

- ✨ Enhance and segment shelf regions
- 🔍 Identify meaningful keypoints and descriptors using ORB  
- 🔗 Match visual features between images of the scene
- 🧹 Post-process bounding boxes to remove duplicates using NMS

This simulates a traditional computer vision pipeline for tasks like **planogram compliance**, **shelf monitoring**, and **product layout analysis**. 📊

## 🖼️ Input Image / Data

The project uses a single image, typically named `retail.jpg`, which shows a supermarket or retail aisle with shelves and products. 🏪

- 📍 The image includes ceiling, floor, shelves, and background elements
- 📦 The shelves are usually arranged in horizontal bands and contain many small textured objects (products)
- 💡 Illumination may vary across the scene, which motivates the use of contrast enhancement and adaptive/binary thresholding

**Note**: If you use a different image or dataset, ensure that the structure is similar (aisle view, visible shelves) for the heuristics and parameters to make sense.

## 🔄 Pipeline Overview

The processing pipeline can be broken down into the following **8 stages**:

1. 📸 Image acquisition and visualization  
2. 🔧 Preprocessing and contrast enhancement  
3. ⚫ Binary segmentation and morphology  
4. 📏 Edge-based region extraction and contour analysis  
5. 🎛️ Heuristic filtering of shelf regions  
6. 🌟 ORB feature extraction on segmented outputs  
7. 🔗 ORB feature matching between images  
8. 🧹 Post-processing of bounding boxes with NMS  

Each stage is designed to refine the representation of the scene and focus more on meaningful structural elements like shelves and product groups. 📈

---

## 1. 📸 Image Acquisition

The pipeline begins by reading the retail image from disk and displaying it.

- **BGR format** (OpenCV default) → **RGB conversion** for correct Matplotlib visualization
- **Sanity check** ensures the image exists (raises error if missing)

**Why this is used**  
✅ Verifies the input and lets you visually confirm that the correct image is being processed before applying any transformations.

---

## 2. 🔧 Preprocessing and Contrast Enhancement

The image is converted to **grayscale** and smoothed with **Gaussian blur**.

- **Grayscale**: Single intensity channel instead of 3 color channels  
- **Gaussian blur**: Reduces noise and stabilizes edges/thresholds
- **CLAHE** (Contrast Limited Adaptive Histogram Equalization) on blurred grayscale

**CLAHE benefits**:
- ✨ Enhances local contrast in different regions
- 🛡️ Prevents over-amplification of noise  
- 📈 Makes shelves/products more distinct from background

**Why this is used**  
Preprocessing improves data quality for thresholding, edge detection, and feature extraction → more reliable segmentation and keypoints.

---

## 3. ⚫ Binary Segmentation and Morphological Operations

**Two segmentation strategies** are used:

### 3.1 Otsu Thresholding
- **Automatic global threshold** based on pixel intensity distribution
- Generates binary mask → **morphological opening** cleans noise and smooths boundaries

### 3.2 Adaptive Thresholding
- **Local thresholds** handle non-uniform lighting
- Often **inverted** so shelves/objects become foreground (white)

**Why this is used**  
Thresholding → binary mask → simplified connected region identification. Morphology cleans up segmentation for robust contour detection.

---

## 4. 📏 Edge-Based Region Extraction

**Edge-based approach**:

- **Canny edge detection** finds gradients around shelf/product boundaries
- **Morphological closing** closes gaps between edges  
- **Dilation** thickens edges → solid regions for contour extraction

**Why this is used**  
Canny + morphology connects fragmented edges → coherent regions approximating shelf geometry.

---

## 5. 🎛️ Contour Analysis and Shelf Region Filtering

Contours extracted → filtered using **geometric heuristics**:

- **Bounding rectangle** → position (x,y), width (w), height (h)
- **Area (w×h)** + **aspect ratio (w/h)** characterize shape

**Filtering rules**:
- ❌ Small areas → noise
- ❌ Top of image → ceiling/background  
- ❌ Bottom → floor
- ❌ Thin regions → noise strips
- ❌ Long thin horizontal → ceiling lamps  
- ❌ Tall narrow → unlikely shelves

**Result**: Green bounding boxes around candidate shelf/product groups.

**Why this is used**  
**Domain knowledge**: Shelves are rectangular, reasonably sized, positioned correctly → reduces false positives.

---

## 6. 🌟 ORB Feature Extraction

**Local feature detection** on segmented images:

- **ORB** (Oriented FAST + Rotated BRIEF) detects keypoints → computes descriptors
- **Tuned parameters**:
  - Limited keypoints → meaningful subset
  - Higher FAST threshold → noise-resistant
  - Focus on **product edges/shelf boundaries**

**Why this is used**  
🔥 **Fast, rotation-invariant**, binary descriptors → real-time retail applications.

---

## 7. 🔗 ORB Feature Matching

**Matching demonstration**:

- ORB features from two images (supports different views)
- **Brute-force matcher** + **Hamming distance** (binary descriptors)
- **KNN matching (k=2)** + **Lowe's ratio test** (0.75 threshold)

**Result**: "Good" matches showing keypoint correspondences.

**Why this is used**  
Fundamental for **image registration, tracking, change detection**. Ratio test ensures reliable matches.

---

## 8. 🧹 Non-Maximum Suppression (NMS)

**Post-processing**:

- Candidate detections → bounding boxes + confidence scores
- **NMS** keeps highest-confidence box → suppresses overlaps (IoU threshold)

**Why this is used**  
🔧 Eliminates redundant overlapping detections → clean final output.

---

## 🎓 What This Project Demonstrates

✅ **End-to-end classical CV pipeline** for retail shelves  
✅ **Preprocessing + heuristics** → structured region segmentation  
✅ **ORB features** → local structure + image correspondences  
✅ **NMS** → refined detection outputs  

---

## 🚀 Possible Extensions

🔄 **Next steps**:

- Replace synthetic NMS with **YOLO/SSD/Faster R-CNN** detections
- Use **different viewpoints** for real ORB matching
- **Generalize heuristics** for various store layouts
- **Modularize** pipeline into functions/classes  

---

🛠️ **Ready to use** - place `retail.jpg` in root → run script → see generated visualizations!
