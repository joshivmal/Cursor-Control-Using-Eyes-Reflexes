# Eye-Controlled Mouse

## Overview

This project demonstrates an eye-controlled mouse cursor using computer vision and machine learning techniques. By tracking eye movements and landmarks on a user's face, the cursor can be controlled to move and perform click actions. This system is particularly beneficial for injured and handicapped individuals who have difficulty using traditional input devices.

## Features

- Real-time eye tracking using a webcam.
- Cursor movement based on eye gaze direction.
- Automated clicking when a specific eye gesture is detected.

## Dependencies

- OpenCV
- MediaPipe
- PyAutoGUI

## Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/eye-controlled-mouse.git
   cd eye-controlled-mouse
   
2. **Install the required libraries**

- Ensure you have Python installed on your system. You can install the necessary libraries using pip:
    ```bash
  pip install opencv-python mediapipe pyautogui


## Usage

1. **Run the Script**
   
- To start the eye-controlled mouse, simply run the eye_controlled_mouse.py script:
  
   ```bash
   python eye_controlled_mouse.py
   
2. **Calibration**

- Position yourself in front of the webcam so that your face is clearly visible.
- Ensure good lighting to improve the accuracy of face and eye tracking.


## How It Works

1. **Face and Eye Detectiont**
   
- The script uses the MediaPipe Face Mesh model to detect facial landmarks.
   It specifically focuses on landmarks around the eyes to track their movements.
   
2. **Cursor Control**

- The coordinates of the eye landmarks are mapped to screen coordinates.
- When the eyes move, the cursor moves accordingly.
- A specific gesture, such as closing the eyes, is interpreted as a click action.


## Code Explanation

The main components of the script include:

- Setting up the webcam feed: The webcam feed is captured using OpenCV.
- Processing each frame: Each frame is processed to detect face landmarks.
- Mapping eye movements to screen coordinates: The eye landmarks are used to move the cursor.
- Detecting eye gestures for clicks: Specific eye gestures trigger click actions.



  ## Key Sections of the Code

 - Initialization

    ```bash
      import cv2
      import mediapipe as mp
      import pyautogui
      
      cam = cv2.VideoCapture(0)
      face_mesh = mp.solutions.face_mesh.FaceMesh(refine_landmarks=True)
      screen_w, screen_h = pyautogui.size()

  - Frame Processing Loop

    ```bash
       while True:
    _, frame = cam.read()
    frame = cv2.flip(frame, 1)
    rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    output = face_mesh.process(rgb_frame)
    landmark_points = output.multi_face_landmarks
    frame_h, frame_w, _ = frame.shape
    if landmark_points:
        landmarks = landmark_points[0].landmark
        for id, landmark in enumerate(landmarks[474:478]):
            x = int(landmark.x * frame_w)
            y = int(landmark.y * frame_h)
            cv2.circle(frame, (x, y), 3, (0, 255, 0))
            if id == 1:
                screen_x = screen_w * landmark.x
                screen_y = screen_h * landmark.y
                pyautogui.moveTo(screen_x, screen_y)
        left = [landmarks[145], landmarks[159]]
        for landmark in left:
            x = int(landmark.x * frame_w)
            y = int(landmark.y * frame_h)
            cv2.circle(frame, (x, y), 3, (0, 255, 255))
        if (left[0].y - left[1].y) < 0.004:
            pyautogui.click()
            pyautogui.sleep(1)
    cv2.imshow('Eye Controlled Mouse', frame)
    cv2.waitKey(1)
## Contributing
Contributions are welcome! If you have any ideas for improvements or have found bugs, 
feel free to open an issue or create a pull request.
