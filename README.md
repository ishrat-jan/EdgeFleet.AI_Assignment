# EdgeFleet.AI_Assignment
Cricket Ball Detection &amp; Tracking System: A complete CV pipeline to detect centroids, track trajectories from a fixed camera, and generate annotated video outputs built for the EdgeFleet.AI Assessment.
This repository contains a complete computer vision system designed to detect and track a cricket ball in videos recorded from a single, fixed camera. The system identifies the ball's centroid, manages visibility states, and overlays the historical trajectory onto the output video.
Key Features
Ball Detection: Per-frame centroid detection ($x, y$ coordinates).
Trajectory Overlay: Generates processed MP4 videos with visual tracking paths.
Data Export: Outputs per-frame annotations in CSV format (frame index, x, y, visibility).
Reproducibility: Includes full scripts for training, inference, and evaluation
