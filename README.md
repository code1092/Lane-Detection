# Lane Detection System

<div align="center">

![Lane Detection](https://img.shields.io/badge/Computer%20Vision-OpenCV-brightgreen)
![Python](https://img.shields.io/badge/Python-3.7+-blue)
![License](https://img.shields.io/badge/License-MIT-green)

An intelligent computer vision solution for detecting and highlighting road lanes in video streams using edge detection and line identification algorithms.

</div>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Key Functions](#key-functions)
- [Output](#output)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

The **Lane Detection System** is a Python-based computer vision project designed to automatically detect and highlight lane markings on roads in video content. This project uses classical image processing techniques including edge detection (Canny), region of interest masking, and Hough line transformation to identify lane boundaries in real-time or batch video processing.

**Perfect for:**
- Autonomous vehicle research
- Driver assistance systems (ADAS)
- Road safety analysis
- Computer vision education
- Traffic monitoring applications

---

## ✨ Features

✅ **Real-time Lane Detection** - Process video frames efficiently  
✅ **Edge Detection** - Advanced Canny edge detection for clear lane identification  
✅ **Region of Interest (ROI)** - Triangular masking to focus on relevant road area  
✅ **Hough Line Transform** - Robust line detection algorithm  
✅ **Video Output** - Generates processed video with detected lanes highlighted  
✅ **Easy to Use** - Simple Jupyter Notebook interface  
✅ **Visualization** - Step-by-step visual output at each processing stage

---

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- **Python 3.7+**
- **pip** (Python package manager)
- **Jupyter Notebook** or **Google Colab**

---

## 🚀 Installation

### Option 1: Local Environment

1. **Clone the repository:**
   ```bash
   git clone https://github.com/code1092/Lane-Detection.git
   cd Lane-Detection
   ```

2. **Install required packages:**
   ```bash
   pip install opencv-python numpy matplotlib jupyter
   ```

3. **Place your video file in the repository directory:**
   - The project uses `road.mp4` by default
   - You can replace it with your own video file

4. **Open Jupyter Notebook:**
   ```bash
   jupyter notebook Untitled0.ipynb
   ```

### Option 2: Google Colab (Recommended for beginners)

1. Open [Google Colab](https://colab.research.google.com)
2. Go to **File** → **Open notebook** → **GitHub**
3. Enter: `code1092/Lane-Detection`
4. Upload your video file to Colab (drag & drop or use `files.upload()`)

---

## 💻 Usage

### Running the Complete Pipeline

1. **Run all cells** in the notebook sequentially
2. The notebook will:
   - Install required dependencies
   - Load and display the input video frame
   - Apply image processing steps
   - Detect lanes using Hough line transformation
   - Generate an output video with highlighted lanes

### Processing Your Own Video

Replace the video file path in the code:

```python
cap = cv2.VideoCapture("your_video.mp4")  # Replace with your video file
```

### Adjusting Detection Parameters

Fine-tune the lane detection by modifying these parameters:

```python
# Canny Edge Detection
edges = cv2.Canny(gray, 50, 150)  # Adjust thresholds: (lower, upper)

# Hough Line Transform
lines = cv2.HoughLinesP(
    masked_edges,
    rho=2,                    # Distance resolution in pixels
    theta=np.pi/180,          # Angle resolution in radians
    threshold=50,             # Minimum votes required
    minLineLength=50,         # Minimum line length
    maxLineGap=150            # Maximum gap between line segments
)

# Region of Interest (ROI) - Adjust triangle coordinates
triangle = np.array([[
    (int(0.1*width), height),      # Left bottom corner
    (int(0.9*width), height),      # Right bottom corner
    (int(0.5*width), int(0.6*height))  # Top center point
]])
```

---

## 🔍 How It Works

The lane detection system follows these key steps:

### **Step 1: Color Space Conversion**
- Converts the input frame from BGR (OpenCV's default) to grayscale
- Reduces complexity and focuses on intensity values

### **Step 2: Edge Detection (Canny Algorithm)**
- Detects edges with 50-150 threshold values
- Produces binary image showing lane boundaries
- Effectively highlights road markings

### **Step 3: Region of Interest (ROI) Masking**
- Creates a triangular mask focusing on the road ahead
- Eliminates unnecessary image regions
- Improves detection accuracy and speed

### **Step 4: Hough Line Transform**
- Identifies straight lines in the masked edge image
- Converts edge points to line parameters (rho, theta)
- Detects lane markings with configurable sensitivity

### **Step 5: Visualization & Output**
- Draws detected lines in green on the original frame
- Blends original frame with detected lanes
- Writes processed frames to output video file

---

## 📊 Project Structure

```
Lane-Detection/
├── README.md                    # This file
├── Untitled0.ipynb             # Main Jupyter notebook with complete pipeline
├── road.mp4                    # Sample input video
└── output_lane.mp4             # Generated output video (created after running)
```

---

## 🛠️ Key Functions

### `process_frame(frame)`
Processes a single video frame through the entire lane detection pipeline.

**Input:** BGR video frame  
**Output:** Frame with detected lanes highlighted in green

```python
def process_frame(frame):
    # Convert to grayscale
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    
    # Edge detection
    edges = cv2.Canny(gray, 50, 150)
    
    # Apply ROI mask
    height, width = edges.shape
    mask = np.zeros_like(edges)
    triangle = np.array([[(int(0.1*width), height), 
                          (int(0.9*width), height),
                          (int(0.5*width), int(0.6*height))]])
    cv2.fillPoly(mask, triangle, 255)
    masked_edges = cv2.bitwise_and(edges, mask)
    
    # Detect lines
    lines = cv2.HoughLinesP(masked_edges, 2, np.pi/180, 50, 50, 150)
    
    # Draw lines
    line_image = np.zeros_like(frame)
    if lines is not None:
        for line in lines:
            x1, y1, x2, y2 = line[0]
            cv2.line(line_image, (x1, y1), (x2, y2), (0, 255, 0), 5)
    
    # Blend with original
    combo = cv2.addWeighted(frame, 0.8, line_image, 1, 1)
    return combo
```

---

## 📹 Output

The system generates:

1. **Intermediate visualizations** showing:
   - Grayscale conversion
   - Edge detection results
   - ROI masking
   - Final lane detection

2. **Output video** (`output_lane.mp4`) with:
   - Detected lanes highlighted in **green**
   - Same resolution and frame rate as input
   - Ready for analysis or further processing

---

## 🚀 Future Enhancements

- [ ] Add color filtering for different lane marking colors
- [ ] Implement curved lane detection (polynomial fitting)
- [ ] Add lane deviation alerts
- [ ] Support for deep learning models (YOLO, U-Net)
- [ ] Real-time processing with live camera feed
- [ ] Advanced filtering to reduce false positives
- [ ] Speed and direction estimation
- [ ] Multi-lane detection and classification

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a new feature branch (`git checkout -b feature/improvement`)
3. **Commit** your changes (`git commit -m 'Add improvement'`)
4. **Push** to the branch (`git push origin feature/improvement`)
5. **Open** a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 📧 Support & Questions

- **Issues:** Found a bug? [Open an issue](https://github.com/code1092/Lane-Detection/issues)
- **Discussions:** Have questions? [Start a discussion](https://github.com/code1092/Lane-Detection/discussions)

---

## 🙏 Acknowledgments

- OpenCV documentation and tutorials
- Computer vision community and researchers
- Image processing algorithms (Canny, Hough Transform)

---

**Made with ❤️ for the computer vision community**

<div align="center">

⭐ If you found this helpful, please star the repository! ⭐

</div>
