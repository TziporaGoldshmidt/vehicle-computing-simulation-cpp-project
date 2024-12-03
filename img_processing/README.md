# Image Processing Team - Vehicle Computing Simulator

This document consolidates all key information related to the **Image Processing** tasks and responsibilities within the **Vehicle Computing Simulator** project. It provides an overview of key classes, functions, algorithms, and techniques used in the system.

---

## Overview

The **Image Processing** module is a critical component of the **Vehicle Computing Simulator**, focusing on the following key objectives:
1. Calculating the relative velocity of detected objects concerning our vehicle.
2. Measuring distances between the vehicle's camera and identified objects.
3. Detecting, tracking, and managing objects in real-time for threat evaluation.
4. Optimizing resource usage to maintain system efficiency without sacrificing detection reliability.

This module leverages advanced techniques such as lane detection, object tracking, and velocity calculation using OpenCV and other algorithms.

---

## Key Components

### **LogManager**
- **Purpose:** Manages and formats log messages for consistent and structured logging.
- **Features:**
  - Uses enum keys to map values to strings for structured log messages.
  - Helps identify and manage similar alerts (e.g., image loading errors).

---

### **Lane Detection**
- **Objective:** Precisely identify the vehicle's lane on the road using OpenCV processing and filtering techniques.
- **Future Use:** Integration with systems to alert drivers about obstacles within the lane.

---

### **Velocity Calculation**
- **Goal:** Accurately calculate the relative velocity of objects.
- **Key Elements:**
  - Uses the formula: `velocity = distance / time difference`.
  - **Smoothing Techniques:** Employs the **Exponential Moving Average (EMA)** algorithm with an alpha value of `0.17` to stabilize velocity readings.

#### **Historical Data Usage**
- Averages recent distance changes to minimize noise and fluctuations in velocity measurements.
- Ensures stability and accuracy by calculating the average of differences in recent distance measurements.

---

### **Distance Measurement**
- **Purpose:** Calculate the distance between the vehicle camera and an object using:
  - Camera focal length.
  - Known size of the object.
  - Image size.

#### **Formula:**
`distance = focal length * known size / image size`

#### **Calibration:**
- Requires a focal length calibration using a picture of a 20 cm black line on a white background, taken from a distance of 1 meter.

---

### **Object Detection and Tracking**
- **IoU (Intersection over Union):**  
  - Metric to evaluate accuracy of bounding boxes from tracking vs. ground truth.
  - **Range:**  
    - `0`: No overlap.  
    - `1`: Perfect overlap.  
    - Values above `0.5` indicate accurate detection.

#### **Efficient Detection**
- Object detection is performed once every 10 frames.
- Intermediate frames use object tracking to reduce computational costs.

#### **Static Scenarios Optimization:**
- When the vehicle is stationary (e.g., at a traffic light), the system:
  - Subtracts the current image from the previous image to detect changes.
  - Sends bounding boxes of changes for further processing.

---

## Performance Optimization

1. **Real-time Efficiency:**
   - Balances computational cost by alternating between detection and tracking.
   - Maintains system reliability with reduced detection frequency.
2. **Reduced Detection During Inactivity:**
   - Implements image subtraction for precise detection when the vehicle is not moving.

---

## Implementation Highlights

1. **Decoding Image Data:**  
   Decodes raw image data from the vehicle's camera for further processing.

2. **Tracker Management:**  
   - Updates object positions based on tracking data.  
   - Removes trackers after repeated failures to maintain data integrity.

3. **Algorithms Used:**  
   - **Exponential Moving Average (EMA):** For velocity smoothing.  
   - **IoU Calculation:** For object tracking accuracy.

---

## Summary

The **Image Processing** team ensures robust, efficient, and accurate processing of camera data for critical functionalities like lane detection, object tracking, and velocity measurement. The system is designed to handle real-time constraints while optimizing computational resources. This enables the seamless integration of features critical for autonomous vehicle simulations.
