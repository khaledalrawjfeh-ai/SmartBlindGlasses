# Smart AI Glasses — Assistive Vision System for the Visually Impaired

> An open-source, independent R&D project exploring how embedded systems, computer vision, and AI can improve environmental awareness and independent navigation for people with visual impairments.

This is a personal engineering project, developed and maintained independently outside of any institutional or academic program. It is documented publicly so the assistive-tech and maker communities can follow, reuse, and contribute to it.

---

## 1. Overview

Visually impaired individuals face daily challenges in detecting obstacles, identifying objects, and navigating unfamiliar environments. Traditional tools like the white cane offer basic obstacle feedback but no contextual understanding of the surroundings.

**Smart AI Glasses** is a wearable prototype that combines ultrasonic distance sensing, onboard image capture, and AI-based object detection to give users both instant collision warnings and spoken descriptions of what's around them.

## 2. Current System (v1 Prototype)

| Capability | Description |
|---|---|
| Obstacle detection | Ultrasonic sensor (HC-SR04) continuously measures distance and triggers buzzer/LED alerts within a danger threshold |
| Real-time object detection | ESP32-CAM captures frames, sent to an external unit running a YOLOv8 model |
| Face recognition | Identifies known faces from a trained dataset |
| Text recognition (OCR) | Reads printed text aloud |
| Currency recognition | Identifies banknotes/denominations |
| Voice interaction | Voice commands and spoken responses |
| GPS navigation | Basic location and route guidance |
| Emergency SOS | Sends alerts/location to a trusted contact |
| Audio feedback | Detected objects converted to Arabic speech (TTS) and played via Bluetooth speaker |
| Mobile app | Companion app for configuration and monitoring |
| Cloud sync | Syncs logs/settings across devices |
| Battery monitoring | Tracks and reports power status |

### Hardware (current)

- ESP32 microcontroller (system control)
- ESP32-CAM module (image capture)
- HC-SR04 ultrasonic sensor (distance measurement)
- Buzzer + LED indicators (immediate alerts)
- Bluetooth speaker (audio output)
- External processing unit / laptop (AI inference)

### Software architecture (current)

```
[ESP32-CAM] --image--> [Processing Unit: YOLOv8 inference]
[HC-SR04] --distance--> [ESP32: threshold check --> buzzer/LED]
[Detection results] --> [TTS engine] --> [Bluetooth speaker]
[GPS module] --> [Navigation logic] --> [Mobile app / audio]
[SOS trigger] --> [Location + alert] --> [Contact notification]
```

The architecture is intentionally modular — sensing, inference, and feedback are decoupled — so new sensors or AI models can be swapped in without redesigning the whole system.

## 3. Roadmap

This repository is organized into staged documentation so the project's direction is transparent and reviewable:

- **[`docs/AI_ROADMAP.md`](docs/AI_ROADMAP.md)** — Near-to-mid-term AI upgrades (LLMs, VLMs, edge AI, scene understanding, semantic mapping, etc.) with realistic implementation notes.
- **[`docs/SENSOR_FUSION.md`](docs/SENSOR_FUSION.md)** — Advanced multi-sensor wearable concepts (thermal, LiDAR, radar, IMU, biosensors) and how AI could fuse them into a coherent picture of the environment.
- **[`docs/RESEARCH_ROADMAP.md`](docs/RESEARCH_ROADMAP.md)** — Long-horizon, experimental research directions (BCI, neural interfaces, sensory substitution), each rated by technology readiness, feasibility, and ethical/safety considerations. This is clearly separated from the shipping product — nothing here is claimed as working technology today.

## 4. Project Status

Prototype / active development. Not a medical device. Not evaluated or certified for clinical use.

## 5. Contributing

Issues and pull requests are welcome — especially from people with lived experience of visual impairment, embedded systems engineers, and AI/ML practitioners. Please open an issue before submitting large changes so we can discuss design direction first.

## 6. License

Choose and add a license (e.g., MIT, Apache-2.0) before publishing — see `LICENSE`.

## 7. Disclaimer

This project is an independent, non-commercial, personal R&D effort. It is not affiliated with any university, employer, or company. Speculative or experimental ideas discussed in the roadmap documents are research directions only, not claims about current product capability.
