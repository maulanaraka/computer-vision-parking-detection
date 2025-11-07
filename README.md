# computer-vision-parking-detection

This project uses YOLO and OpenCV to detect and monitor parking spot occupancy in real-time from a video feed.

The core logic is a "3-phase" approach:
1. Config: Load all required models and video sources.
2. Calibration: Dynamically learn the locations of all parking spots from the video.
3. Detection: Use the calibrated spots to check for occupancy, demonstrating three different detection methods.

# Features

- Dynamic Spot Calibration: Automatically detects and defines parking zones using a YOLO-OBB (Oriented Bounding Box) model. This means you don't need to manually label parking spot coordinates.
- Method 1: Base YOLO Detection: Uses a standard YOLO model to detect cars (axis-aligned boxes) and checks if the car's center point is inside a calibrated parking zone.
- Method 2: OBB YOLO Detection: Uses a YOLO-OBB model to detect cars (rotated boxes) for a more accurate center-point calculation.
- Method 3: Custom OBB Model (End-to-End): Uses a single, custom-trained YOLO-OBB model that was taught to classify parking spots as "space-empty" or "space-occupied" directly.
- Temporal Smoothing: The final method includes a temporal buffer to smooth out detection "flickering," providing a more stable count.
- Real-time Visualization: Displays the video feed with colored polygons (green for empty, red for occupied) and a live count of available spots.

# Hands on Tutorial

1. Setup
    1. Clone the repository  
        'git clone <https://github.com/maulanaraka/computer-vision-parking-detection.git>'  
        'cd <computer-vision-parking-detection>' or folder location
    2. Create Virtual environment  
        'python -m venv .venv'  
        'source .venv/bin/activate'  # On Linux/macOS  
        ''.\.venv\Scripts\activate'   # On Windows
    3. Install dependencies  
        'pip install -r requirements.txt'
2. Running the ecode
    Run the first three cells:
    - Config: This loads all libraries and model paths.
    - Calibration (Code Cell 1): This defines settings and helper functions.
    - Calibration (Code Cell 2): This is the most important step. It will:
        Open a video window.  
        Analyze the first 60 frames (CALIBRATION_FRAMES) of the video.  
        Draw yellow-to-green boxes as it "learns" and stabilizes the parking spot locations.  

    Once finished, it stores all stable spots in the stable_parking_slots variable.  

    After calibration, the stable_parking_slots are in memory. You can now run any one of the three detection methods.
    1. Option A: Run "YOLO BASE MODEL"
        This method uses a standard car detector.  
        It's the simplest but may be less accurate if cars park at an angle.  
        Run the cell under the YOLO BASE MODEL heading.  

    2. Option B: Run "YOLO OBB MODEL"
        This method uses a rotated car detector (OBB).  
        It's more accurate for calculating the car's center, especially in angled parking.  
        Run the cell under the YOLO OBB MODEL heading.  

    3. Option C: Run "CUSTOM TRAIN YOLO OBB" (Recommended)
        This is the most advanced method. It uses your custom model (parking-obb2...best.pt) that directly classifies spots.  
        It is the most robust as it includes temporal smoothing.  
        Run the cell under the CUSTOM TRAIN YOLO OBB heading.  

    A window will open showing the real-time detection, with spots colored green (Empty) or red (Occupied) and a live counter. Press 'q' to quit the video feed.
