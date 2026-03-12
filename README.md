# FACIAL PRINT v0.0.5.3

**FACIAL PRINT** is an experimental real-time expression-reactive particle rendering system that maps human facial dynamics to audiovisual landscapes. 

Created by **Qicheng Dai**, this project explores the intersection of computer vision, generative art, and interactive music.

---

## 📽️ Project Summary

The application uses your webcam to detect 468+ facial landmarks in real-time. These landmarks are transformed into a dense cloud of particles that respond to your movements, expressions, and emotions. Simultaneously, a custom-built Web Audio engine generates a synchronized soundtrack that shifts its melody, rhythm, and texture based on your "warmth" (smile), "activity" (movement), and "openness" (mouth).

---

## 🛠️ Technical Implementation

### 1. Facial Perception (MediaPipe Face Mesh)
- **Landmark Detection**: Uses Google's MediaPipe to track high-fidelity facial geometry. 
- **Normalization**: To ensure stability regardless of the user's distance from the camera, the system normalizes distances using a reference vector (e.g., the 3D width of the face).
- **Expression Derivation**:
    - `exprSmile`: Calculated by the horizontal stretch of the mouth corners relative to face width.
    - `mouthOpen`: Measured by the vertical gap between the lips.
    - `isSleeping`: Detected using the **Eye Aspect Ratio (EAR)**; if eyes remain closed for >2 seconds, the system enters a "sleep" state, slowing down the music BPM.
- **Pose Estimation**: Tracks Pitch, Yaw, and Roll to drive "Motion Magnitude," which influences the music's complexity and distortion.

### 2. Generative Visuals (Canvas 2D Particle System)
- **Dot Cache**: To maintain high performance, particles are not generated every frame. Instead, a static 'dot cache' is built around landmarks, and their positions are updated based on the current face mesh deformation.
- **Styling & Blending**: 
    - Five distinct themes (**INK, DUST, AMBER, AURORA, VELVET**) define the particle behavior (trail length, color mapping, density).
    - **Gradual Style Blending**: Switching styles triggers a linear interpolation of HSLA properties, creating smooth transitions between visual moods.
- **Optimal Clipping**: Uses a custom `clipOval` function to ensure particles only render within the natural silhouette of the face, creating a "holographic mask" effect.

### 3. Audiovisual Synthesis (Web Audio API)
- **Look-ahead Scheduler**: A high-precision sequencer pattern that schedules audio events slightly ahead of time to ensure perfect synchronization, overcoming the jitter typical of `setTimeout` or `setInterval`.
- **Expression-to-Music Mapping**:
    - **Harmonic Modes**: Expressions trigger shifts between musical modes (Dorian → Mixolydian → Major → Lydian). Smile level directly correlates to "brightness."
    - **Dynamic BPM**: Head movement speed (Motion Magnitude) dynamically increases the BPM from 80 up to 116.
    - **Granular SFX**: Simple but evocative sounds for clicks and shutter transitions formed using Sine waves and filtered White Noise.
- **Waveform Visualization**: A pixelated particle analyser in the top bar provides real-time feedback of the master gain output.

### 4. Cinematic Capture System
- **Triple Burst**: Sequentially captures 3 frames using different user-selected styles with a countdown.
- **Video Recording**: Uses the `MediaRecorder API` to capture a 15-second high-definition clip directly from the canvas.
- **Filmstrip Processor**: A custom composition engine that takes captured frames and wraps them in a stylized cinema filmstrip, complete with sprocket holes, edge markings, and customizable hue tints.

---

## 🚀 Getting Started

1.  Open `facial-print-v0.0.5.3.html` in any modern web browser (Chrome/Edge/Safari).
2.  Allow camera access.
3.  Click the **SOUND [OFF]** button to initialize the audio context.
4.  **Interact**:
    - **Smile**: Brightens the music and visual colors.
    - **Move**: Increases the tempo and visual energy.
    - **Tilt**: Distorts and modulates the sound textures.
    - **Capture**: Use the right-side button to save your "Facial Print" as a photo or video.

---

## 📜 License

© 2026 Qicheng Dai. All Rights Reserved. 
The code is provided for educational and portfolio demonstration purposes. Link to [Original Repository](https://github.com/dominoghm123/facialprint).