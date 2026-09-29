# Project Report: Chassis Structural Integrity Monitoring

This notebook demonstrates a simulated system for monitoring chassis structural integrity using vibration analysis. The core idea is to detect potential structural issues, such as cracks or loose joints, by analyzing specific frequency patterns in vibration data collected from sensors.

## Key Components and Demonstrations:

### 1. Raw Data Simulation and Structural Defect Introduction

*   **Purpose**: To mimic real-world sensor data from a vehicle's chassis and introduce a simulated structural defect.
*   **Methodology**: Vibration data is generated over a 2-second period with a sampling frequency of 1000 Hz.
    *   `normal_vibration`: Simulates ambient vibrations from typical vehicle operation (engine hum, road noise).
    *   `crack_signature`: A high-frequency sinusoidal wave (120 Hz) is added to represent the unique vibration signature of a structural defect.
*   **Visualization**: The "Raw Accelerometer Data (Time Domain)" plot (top graph in the initial output) shows this combined signal, appearing chaotic due to the mix of normal and defect-induced vibrations.

### 2. Microcontroller Data Processing (Vibration Analysis)

*   **Function**: `analyze_chassis_vibration(signal, fs, threshold)`
*   **Process**: This function simulates the processing capability of a microcontroller:
    1.  **Fast Fourier Transform (FFT)**: Applied to the raw time-domain signal to convert it into the frequency domain. This allows for the identification of specific vibration frequencies and their magnitudes.
    2.  **High-Frequency Pattern Isolation**: The system focuses on frequencies above 100 Hz, as structural defects often manifest as higher-frequency vibrations.
    3.  **Threshold Comparison**: The maximum magnitude of these high-frequency components is compared against a predefined `safety_threshold` (e.g., 0.8).
*   **Visualization**: The "Vibration Analysis (Frequency Domain)" plot (middle graph) clearly shows frequency spikes. A prominent spike around 120 Hz indicates the detected defect. The red dashed line visually represents the `safety_threshold`.

### 3. Safety Telltale Indicator

*   **Purpose**: To provide a clear and immediate warning to the user or driver.
*   **Mechanism**: If the maximum high-frequency vibration magnitude crosses the `safety_threshold`, a visual alert is triggered.
*   **Visualization**: The "Safety Telltale Indicator" (bottom display) shows either a '⚠️ WARNING: CHASSIS STRESS DETECTED ⚠️' message in red or '✅ CHASSIS STRUCTURAL INTEGRITY NORMAL' in green, depending on the analysis result.

### 4. Noise Sensitivity Testing

*   **Purpose**: To evaluate the robustness of the system's detection capabilities under varying levels of environmental noise.
*   **Methodology**: An additional, tunable `noise_amplitude` parameter (`0.3` by default) is introduced to generate random noise, which is then added to the sensor data.
*   **Experimentation**: By adjusting the `noise_amplitude`, users can observe how the system's ability to detect the crack signature changes. This helps in calibrating the `safety_threshold` to balance sensitivity (detecting true defects) and robustness (avoiding false positives from noise).
*   **Observation**: The plots generated during noise simulation illustrate how the raw data becomes more erratic, but the FFT still isolates the critical 120 Hz spike, and the telltale indicator updates based on whether this spike (with noise) still crosses the threshold.
