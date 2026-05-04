# AMET AR Tag Final Drive

Python ROS node for AprilTag-based autonomous driving during the final AR-tag section of the 2025 AMET MAZE hackathon.

This project controls an Xycar-class vehicle with a USB camera, detects the closest AprilTag, estimates distance and lateral offset, and adjusts steering to align the vehicle with a target path through the AR-tag zone.

## Overview

The script subscribes to camera frames from ROS, detects AprilTags with OpenCV and `apriltag`, and publishes steering and speed commands through `xycar_motor`.

Core behavior:

- Detect the nearest visible AprilTag.
- Estimate forward distance from tag size and camera calibration values.
- Estimate lateral offset from the tag center position.
- Switch into AR-drive mode once the vehicle reaches the configured trigger distance.
- Adjust steering in fixed steps to keep the vehicle near the desired offset.
- Reuse the previous steering angle for a short time if the tag is temporarily lost.

## Tech Stack

- Python
- ROS (`rospy`)
- OpenCV
- `apriltag`
- `cv_bridge`
- `xycar_msgs`
- `sensor_msgs`

## File Structure

```text
.
|- ar_drive.py
`- README.md
```

## How It Works

1. Capture camera images from `/usb_cam/image_raw`.
2. Convert the image to grayscale and detect AprilTags.
3. Choose the closest detected tag.
4. Estimate forward distance from tag size in pixels and lateral displacement from the image center.
5. Enter AR-drive mode when the vehicle is close enough to the target tag.
6. Update steering angle to reduce the gap between the current offset and the desired reference offset.
7. Publish steering and speed commands to `xycar_motor`.

## Requirements

- ROS environment with Python support
- Camera topic available at `/usb_cam/image_raw`
- Vehicle command topic compatible with `xycar_motor`
- Installed Python packages and ROS dependencies

```bash
pip install apriltag opencv-python numpy
```

You will also need the ROS packages required by `rospy`, `cv_bridge`, `sensor_msgs`, and `xycar_msgs`.

## Running

Run the node inside your ROS environment after confirming that camera and motor topics match your setup.

```bash
python ar_drive.py
```

## Tunable Parameters

The current script includes hard-coded values that should be re-tuned for your vehicle and camera setup:

- `std = 20`
- `f_std = 100`
- `tag_size = 9.5`
- `camera_matrix`
- steering step size
- speed value

## Limitations

- The code is tightly coupled to an Xycar-style ROS environment.
- Camera calibration values are specific to the original hardware setup.
- Topic names and motor message types may need adjustment for other platforms.
- Steering logic is rule-based and tuned for a specific competition course.

## Publication Note

Only publish source files and assets that you have permission to redistribute. Third-party starter code, training materials, or competition-provided files may have separate licensing restrictions.
