# 🎥 AI-Powered People Counting & Foot-Traffic Analytics

An advanced Computer Vision system that performs real-time detection, tracking, and bidirectional counting of individuals in a monitored space. This project utilizes **YOLOv11** for high-precision detection and **Polygon ROI (Region of Interest)** logic to determine entry and exit events.

---

<p align="center">
  <img src="assets/project_output-optimize.gif" alt="YOLOv11 People Detector Output" width="800">
</p>

---

## 🌟 Features
-----------

-   **Real-time Object Tracking:** Implements the `persist=True` parameter in YOLOv11 to maintain unique identities (IDs) for every person in the frame.
-   **Bidirectional Counting:** Uses dual-polygon logic to distinguish between people entering and exiting a premises.
-   **Live Occupancy Analytics:** Dynamically calculates the current number of people inside based on `Entered - Exited`.
-   **Visual Debugging:** Renders bounding boxes, unique track IDs, and counting boundaries directly onto the video stream.
-   **Automated Export:** Processed footage is automatically saved with all overlays (counters, polygons, boxes) using the `VideoWriter` module.

## 🛠️ Technical Stack
-------------------

-   **Core Engine:** YOLO11 (Ultralytics) for object detection.
-   **Processing:** Python, NumPy (for coordinate handling).
-   **Vision & UI:** OpenCV (cv2) for video I/O and CVZone for stylized text/UI overlays.
-   **Geospatial Logic:** `cv2.pointPolygonTest` to accurately detect when a person's center-point crosses specific spatial boundaries.

## 📐 Algorithmic Logic
--------------------

The system defines two specific polygonal areas (`area1` and `area2`) placed near a doorway or threshold:

1.  **Detection:** The model identifies a `person` and assigns a `track_id`.
2.  **Point of Interest:** The system tracks the bottom-center coordinate $(x_1, y_2)$ of the bounding box---the point where the person's feet touch the ground.
3.  **Cross-Verification:**
    -   **Entry:** If a `track_id` is first detected in `area2` and subsequently moves into `area1`, the system increments the **Entered** counter.
    -   **Exit:** If a `track_id` is first detected in `area1` and subsequently moves into `area2`, the system increments the **Exited** counter.
4.  **State Management:** Python dictionaries (`enter` and `exits`) store the initial state of each unique ID to prevent double-counting.

## 🚀 Setup & Installation
-----------------------

1.  **Clone the repository:**

    Bash

    ```
    git clone https://github.com/[Your-Username]/people-counting-yolo11.git
    cd people-counting-yolo11

    ```

2.  **Install dependencies:**

    Bash

    ```
    pip install ultralytics opencv-python cvzone numpy

    ```

3.  **Run the application:**

    Bash

    ```
    python main.py

    ```

## 📊 Configuration
----------------

You can customize the detection area by modifying the polygon coordinates in `main.py`:

Python

```
area1 = [(250, 444), (211, 448), (473, 575), (514, 566)]
area2 = [(201, 449), (177, 453), (420, 581), (457, 577)]

```

*Use the integrated RGB mouse callback feature to find the exact $(x, y)$ coordinates for your specific camera angle.*

## 🎥 Output Example
-----------------

The system generates an `output_people_count.mp4` file containing:

-   Real-time counters for **Entered**, **Exited**, and **Current** occupancy.
-   Unique Tracking IDs for every detected individual.
-   Visual boundaries (Pink polygons) showing the active detection zones.



