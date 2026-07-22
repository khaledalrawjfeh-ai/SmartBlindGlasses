# Advanced Sensor Fusion — Research Notes

Goal: expand beyond the current ultrasonic + single camera setup toward a richer, fused picture of the environment, while keeping output methods safe and non-invasive (speech, spatial audio, vibration).

## 1. Candidate Sensors

| Sensor | What it adds | Wearable-scale availability | Notes |
|---|---|---|---|
| RGB camera (current) | Object identity, text, color | Mature, in use | Baseline |
| Depth camera (stereo or ToF) | Distance to every pixel, not just one ultrasonic beam | Available in small modules (e.g., ToF sensors like VL53L-series, small stereo modules) | Much richer than single-point ultrasonic reading |
| Thermal camera | Detects people/animals in low light or smoke; body-heat cues | Miniature modules exist (e.g., Lepton-class sensors) but are relatively costly | Useful for low-visibility conditions |
| LiDAR (short-range solid-state) | Accurate distance/shape mapping in a wide field | Small automotive/robotics-grade units are shrinking and becoming wearable-feasible | Higher cost and power than ultrasonic |
| mmWave radar | Works through light rain/fog/dust; detects motion and rough distance even in poor visibility | Small radar modules exist (e.g., automotive/robotics mmWave boards) | Good complement to camera in bad weather |
| Ultrasonic (current) | Cheap, reliable short-range distance | Mature, in use | Keep as the "always-on" safety layer — simplest and most power-efficient |
| IMU (accelerometer/gyro) | Head/body orientation, motion, fall detection, step tracking | Mature, tiny, cheap | Also enables gesture-based control |
| Eye tracking | Only relevant if any residual vision/gaze direction is meaningful for the specific user | Niche wearable modules exist | Optional, user-dependent |
| Microphones (array) | Sound source localization (approaching vehicle, someone calling the user's name) | Small MEMS mic arrays are cheap and available | Also enables voice commands |
| Environmental sensors (temp/humidity/air quality/light level) | Context (e.g., adjust behavior for bright sun vs. dark room), general awareness | Mature, cheap | Lower priority, "nice to have" |
| EEG/EMG (biosensors) | Wearer's own physiological/attention state — see caveats in `RESEARCH_ROADMAP.md` | Consumer-grade available, but noisy for active use | Research-stage for this application, not near-term |

## 2. Fusion Approach

A practical, staged fusion architecture:

1. **Safety-critical layer (lowest latency, simplest logic):** Ultrasonic + IMU only. This layer must never depend on AI inference completing — it should always be able to trigger an immediate buzzer/vibration alert.
2. **Perception layer:** Camera + depth (+ thermal/radar if added) feed the object-detection and distance-estimation models. Runs as fast as hardware allows, but is allowed slightly higher latency than layer 1.
3. **Context layer:** GPS/indoor localization + microphone array + environmental sensors feed slower-changing context (where am I, what's the ambient situation) used for route planning and the AI assistant/LLM layer, not for split-second alerts.

Fusing sensors that disagree (e.g., camera doesn't see an obstacle but radar does, in fog) should default to the more cautious reading — false alarms are a much smaller cost than missed obstacles for this use case.

## 3. Output/Feedback Design (Non-Invasive Only)

All output stays through established, safe channels:

- **Speech (TTS):** Best for descriptive, non-urgent information ("a chair is two meters ahead").
- **Spatial/3D audio cues:** Best for direction — a tone that seems to come from where the obstacle actually is, faster to parse than a sentence.
- **Vibrotactile patterns:** Best for urgent, silent alerts (e.g., in loud environments, or when the user prefers discretion) — different vibration patterns/locations can encode distance and direction.
- **No electrical or magnetic brain stimulation of any kind** is used or planned for output — see `RESEARCH_ROADMAP.md` for why that direction isn't currently viable or scientifically supported for this purpose.

## 4. Open Engineering Questions

- Power budget: adding depth/thermal/radar sensors increases draw significantly — battery life vs. capability trade-off needs real measurement, not estimation.
- On-wearable compute vs. offload: which fused sensors can be processed on-device (edge AI) vs. need a paired phone/compute puck.
- Form factor: where physically to mount additional sensors (frame, temple, separate chest unit) without making the glasses impractically heavy or conspicuous.
- Calibration: multi-sensor extrinsic calibration (aligning camera, depth, radar coordinate frames) adds real engineering overhead as more sensors are added.
