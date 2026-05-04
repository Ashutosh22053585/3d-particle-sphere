# Kinetic Sphere - AI Hand Tracking

An interactive 3D particle sphere that reacts to your hand gestures in real-time. Built entirely in a single HTML file without any heavy frameworks, combining the power of **Three.js** for WebGL rendering and **MediaPipe** for advanced AI hand tracking.

## ✨ Features

- **Fluid 3D Particle System**: A beautifully animated sphere consisting of 3000 particles arranged in a golden ratio spiral, surrounded by 800 ambient background dust particles.
- **Single Hand Control**: Move your hand to guide the sphere across the screen. The sphere subtly tilts based on your palm position.
- **Pinch-to-Explode**: Pinch your thumb and index finger together to charge up a glowing ring. Once fully charged (progress > 70%), release to trigger a massive shockwave and scatter the particles, which then reform using spring physics.
- **Dynamic Color Shifting**: Raise different numbers of fingers (0 to 5) to instantly cycle the sphere through premium neon color palettes (White, Cyan, Red, Blue, Orange, Purple).
- **Two Hands (Clap & Scale)**:
  - **Scale**: Move your hands further apart or closer together to scale the sphere up and down dynamically.
  - **Clap Burst**: Bring both hands together quickly to trigger an explosive shockwave effect and blast the particles away.
- **Minimalist Dark UI**: Includes a clean, frosted-glass interface with a live status indicator and an option to overlay the webcam feed in the background.

## 🛠️ Technology Stack

- **HTML5 & Vanilla CSS**: For the structure and styling of the premium dark UI.
- **[Three.js](https://threejs.org/)**: Powers the 3D WebGL particle system, physics, and rendering.
- **[MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands.html)**: Google's machine learning framework used to detect and track 21 3D landmarks of your hands in real-time from the webcam.

## ⚙️ How It Works (The Workflow)

1. **Initialization**: When the page loads, it requests webcam access. The MediaPipe model is downloaded via CDN and initializes. During this time, a loading screen is displayed.
2. **Video Feed Processing**: The webcam feed is continuously passed to MediaPipe. The video is rendered off-screen (or visible with low opacity if toggled).
3. **Landmark Detection**: MediaPipe processes the frames and returns an array of coordinates representing the joints of any detected hands.
4. **Data Smoothing**: To prevent the 3D sphere from jittering, the raw coordinates from MediaPipe are passed through a custom `Smoother` class which uses an exponential moving average.
5. **Gesture Interpretation**:
   - **Position mapping**: The 3D coordinates of the palm (`landmark 9`) are translated into 3D world space coordinates for Three.js.
   - **Pinch detection**: Distance between the thumb tip (`landmark 4`) and index tip (`landmark 8`) is calculated. Hysteresis is applied to ensure a stable lock.
   - **Finger counting**: Compares the Y-coordinates of finger tips to their respective PIP joints to determine if they are raised.
   - **Clap detection**: Calculates the delta velocity of the distance between two palms frame-over-frame.
6. **Rendering Loop**: `requestAnimationFrame` continuously updates the Three.js scene. It interpolates target positions, scales, and colors for buttery smooth transitions, applies physics if an explosion was triggered, and renders the frame.

## 🚀 How to Run Locally

Since this project requires camera access, modern browsers require it to be served over `localhost` or `HTTPS`. 

1. Ensure you have Node.js installed.
2. Open your terminal in this directory.
3. Run a local development server:
   ```bash
   npx http-server -p 8080
   ```
4. Open your browser and navigate to `http://localhost:8080`.
5. Allow camera access when prompted and enjoy!
