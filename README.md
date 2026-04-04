# EdgeFleet.AI_Assignment
Cricket Ball Detection &amp; Tracking System: A complete CV pipeline to detect centroids, track trajectories from a fixed camera, and generate annotated video outputs built for the EdgeFleet.AI Assessment.
This repository contains a complete computer vision system designed to detect and track a cricket ball in videos recorded from a single, fixed camera. The system identifies the ball's centroid, manages visibility states, and overlays the historical trajectory onto the output video.
Key Features
Ball Detection: Per-frame centroid detection ($x, y$ coordinates).
Trajectory Overlay: Generates processed MP4 videos with visual tracking paths.
Data Export: Outputs per-frame annotations in CSV format (frame index, x, y, visibility).
Reproducibility: Includes full scripts for training, inference, and evaluation

# Cricket Ball Detection & Tracking System

## Overview
A complete computer vision pipeline to detect and track a cricket ball in single-camera
broadcast videos. Outputs per-frame CSV annotations and processed MP4 videos with
trajectory overlay.
README.md
requirements.txt

## Setup

### Install dependencies
pip install -r requirements.txt

### requirements.txt includes
ultralytics
sahi
opencv-python
torch
pandas
numpy
roboflow
ffmpeg-python

ffmpeg must also be installed and available on PATH.

## Running the Pipeline

### 1. Training (optional — weights provided)
Open code/train.ipynb and run all cells.
Pulls 9 Roboflow datasets, remaps classes, merges, and fine-tunes YOLOv8x for 45 epochs.

### 2. Inference
Open code/inference.ipynb and run all cells.
Place test videos in the configured INPUT_DIR.
Outputs are written to:
  - output/csv/<video_name>.csv
  - output/video/<video_name>_overlay.mp4

## Output Format

### Annotation CSV
frame, x, y, visible
0, 1284.3, 620.7, 1
1, 1291.0, 628.2, 1
2, -1, -1, 0

visible=1 → ball detected; x/y = centroid in pixels
visible=0 → ball not visible; x/y = -1

### Processed Video
MP4 with ball centroid dot and fading trajectory trail overlayed per frame.
Yellow = real detection. Cyan = Kalman-predicted or interpolated position.

## Model

Base model : YOLOv8x (COCO pretrained)
Fine-tuned on 9 merged Roboflow cricket ball datasets (~5,700 images)
Image size  : 1280px
Epochs      : 45
Optimizer   : AdamW with cosine LR

## Key Design Decisions

- SAHI tiled inference (640px slices, 20% overlap) for high-resolution frames
- Physics-aware Kalman tracker with gravity and drag priors
- Three-state tracker: INIT → TRACKING → LOST
- Dynamic displacement gate: widens for fast deliveries, tightens when ball is slow
- False positive suppression: exclusion zones + prescan blacklist + size + aspect ratio filters
- Linear gap interpolation for gaps ≤5 frames (150px spatial jump guard)
- Dual-GPU inference via ThreadPoolExecutor

## Hardware
Trained and tested on Kaggle dual-T4 GPU environment.
Inference falls back to single GPU or CPU if needed.
