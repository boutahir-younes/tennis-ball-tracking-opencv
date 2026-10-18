# Real-Time Tennis Ball Tracking with OpenCV: CSRT and KCF Trackers and HSV Color Segmentation

A computer vision application that tracks a tennis ball in video using two complementary approaches: object trackers (CSRT and KCF) initialized manually, and fully automatic detection based on HSV color segmentation. The results are displayed side by side in a desktop interface.

![Python](https://img.shields.io/badge/Python-3.13-blue)
![OpenCV](https://img.shields.io/badge/OpenCV-4.13-green)
![GUI](https://img.shields.io/badge/GUI-Tkinter-orange)

---

## Table of Contents

1. [Demo](#demo)
2. [Features](#features)
3. [How It Works](#how-it-works)
4. [Tracking Methods](#tracking-methods)
5. [Tracker Comparison](#tracker-comparison)
6. [User Interface](#user-interface)
7. [Installation](#installation)
8. [Usage](#usage)
9. [Project Structure](#project-structure)
10. [Limitations](#limitations)

---

## Demo

[Watch the demo video](https://github.com/boutahir-younes/Tracker_OpenCV/raw/main/Videos/demo.mp4)

## Features

- Two tracking modes in one application: manual initialization and automatic color-based detection
- Comparison of two OpenCV trackers, CSRT and KCF
- Display of the ball trajectory over time
- Three synchronized video panels showing the original video and both tracking results
- Graphical interface for selecting a video and starting or stopping the tracking

## How It Works

The automatic detection pipeline processes each video frame in seven steps.

| Step | Operation | Purpose |
|---|---|---|
| 1 | Frame extraction | Decompose the video into individual frames |
| 2 | Gaussian filter | Reduce image noise |
| 3 | BGR to HSV conversion | Separate color information from brightness |
| 4 | HSV thresholding | Build a binary mask of the ball color |
| 5 | Morphological operations | Erosion and dilation to remove noise and fill gaps |
| 6 | Contour extraction | Find the ball and compute its bounding box |
| 7 | Tracker initialization | Start the CSRT or KCF tracker on the detected region |

## Tracking Methods

| Method | How it works | Main advantage |
|---|---|---|
| Manual (region of interest) | The user draws a rectangle around the ball in the first frame | Works with any object, not only tennis balls |
| HSV color detection | The yellow-green color of the ball is detected automatically | No user input required |

## Tracker Comparison

| Tracker | Approximate speed | Accuracy | Use in this project |
|---|---|---|---|
| CSRT | about 25 fps | Very high | Recommended, used for the main tracking panel |
| KCF | about 60 fps | Good | Faster alternative, kept for comparison |

CSRT (Discriminative Correlation Filter with Channel and Spatial Reliability) is more accurate but slower. KCF (Kernelized Correlation Filter) is faster but less robust. The speeds above are approximate and depend on the hardware and the video resolution.

## User Interface

The application window contains three panels that run simultaneously.

| Panel | Content |
|---|---|
| Left | Original video |
| Center | CSRT tracking with manual initialization, and ball trajectory |
| Right | Automatic HSV color tracking |

## Installation

```bash
pip install opencv-contrib-python numpy Pillow
```

The `opencv-contrib-python` package is required because the CSRT and KCF trackers are part of the OpenCV contrib modules.

## Usage

Start the application:

```bash
python app.py
```

Then:

1. Select a video from the drop-down menu.
2. Click **LAUNCH**.
3. Draw a rectangle around the ball and press **ENTER** to confirm.
4. The tracking starts on the three panels at the same time.
5. Click **STOP** to end the tracking.

## Project Structure

```text
tennis-ball-tracking-opencv/
├── app.py              # Graphical interface with three panels
├── utils.py            # Image processing pipeline
└── Videos/
    ├── input_1.mp4     # Test video 1
    ├── input_3.mp4     # Test video 3
    └── demo.mp4        # Demo recording
```

## Limitations

- The HSV thresholds are tuned for a yellow-green ball. Strong changes in lighting or a different ball color require adjusting them.
- Fast motion, motion blur and temporary occlusion of the ball can make the trackers lose the target.
- Manual tracking requires the user to select the ball in the first frame.
