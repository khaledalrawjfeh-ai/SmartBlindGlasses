# AI Roadmap — Near-to-Mid-Term Intelligence Upgrades

This document lists realistic, buildable AI upgrades for the Smart AI Glasses platform. Each item includes an implementation approach, hardware implications, key challenges, and how it scales. These are technologies that exist today in some form — the work is integration and adaptation, not invention.

---

## 1. Multimodal AI / Vision-Language Models (VLMs)

**What it adds:** Instead of just naming objects ("chair", "person"), a VLM can answer free-form questions about a scene — "is the door open?", "how many steps ahead?", "what does that sign say?".

- **Approach:** Run a compact VLM (e.g., a distilled/quantized model in the LLaVA or Moondream family) either on-device (if hardware allows) or on a paired phone/edge box; feed it the camera frame plus the user's spoken question.
- **Hardware:** Needs more compute than YOLO alone — a phone-class NPU or a small edge accelerator (e.g., Jetson-class board) rather than a microcontroller.
- **Challenges:** Latency (VLM inference is slower than a detector), hallucination risk (must not confidently invent details that affect safety), power draw.
- **Scalability:** Start with cloud-based VLM calls for prototyping, migrate to on-device quantized models as they mature, to remove the internet dependency called out in the current scope.

## 2. Large Language Models (LLMs) as a Conversational Layer

**What it adds:** A natural-language assistant that can hold context ("remind me where I parked", "what did I just walk past?") rather than only reacting to single detections.

- **Approach:** LLM as an orchestration layer that receives structured events from the vision/sensor pipeline (object list, distances, GPS) and turns them into natural spoken summaries, plus handles user queries.
- **Hardware:** Cloud API call is simplest; on-device small LLMs (1–3B parameters, quantized) are viable on modern phone SoCs for offline fallback.
- **Challenges:** Response latency for real-time alerts must stay separate from the LLM path — safety-critical obstacle warnings should never wait on an LLM round-trip.
- **Scalability:** Layered design — fast reactive layer (ultrasonic + detector) always active; LLM layer is an optional "ask for more detail" mode.

## 3. Edge AI

**What it adds:** Removes dependence on a laptop/cloud for inference, improving latency, privacy, and reliability in areas with poor connectivity.

- **Approach:** Quantize the YOLO model (INT8/TFLite or ONNX Runtime Mobile) and deploy to an edge accelerator (e.g., ESP32-S3 with a co-processor, a Raspberry Pi + Coral TPU, or a phone NPU via the mobile app).
- **Hardware:** A dedicated compute module is likely needed beyond the current ESP32-CAM, since camera + inference on one microcontroller is tight.
- **Challenges:** Model accuracy trade-offs from quantization; thermal/power budget on a wearable.
- **Scalability:** Natural next step after the current prototype — directly addresses the "requires internet connectivity" limitation already noted in the project scope.

## 4. Scene Understanding & Semantic Mapping

**What it adds:** Beyond single-frame object detection — building a running model of "this is a hallway with a door on the left and stairs ahead" rather than isolated labels.

- **Approach:** Combine object detection with depth estimation (monocular depth model or a depth sensor) and simple SLAM-style tracking to build a lightweight semantic map of the immediate area.
- **Hardware:** Benefits from a depth sensor (see `SENSOR_FUSION.md`) but can start with monocular depth estimation from the existing camera.
- **Challenges:** Computationally heavier than detection alone; drift/accuracy in fast-changing environments.
- **Scalability:** Feeds directly into indoor localization and route planning (below).

## 5. Predictive Navigation & Intelligent Route Planning

**What it adds:** Suggests routes that avoid known problem areas (stairs, construction, crowded zones) rather than only reacting to what's directly ahead.

- **Approach:** Combine GPS/indoor localization with a routing engine (e.g., OSRM or a custom graph) and a simple hazard-scoring layer built from crowd-sourced or user-logged hazard reports.
- **Challenges:** Requires either crowd-sourced data or long-term personal-use data to be useful; cold-start problem for new areas.
- **Scalability:** Can start purely personal (routes the user has walked before) before any shared/community data layer is considered.

## 6. AI Memory of Frequently Visited Locations

**What it adds:** The system recognizes "you're near your usual bus stop" or "this is your workplace entrance" and proactively offers relevant info.

- **Approach:** On-device embedding/clustering of GPS + visual landmarks for frequently visited places; simple recency/frequency-based memory, not raw video storage.
- **Privacy note:** This should be stored locally and be user-deletable, not synced to a shared cloud by default — location history is sensitive.

## 7. Human Activity Recognition

**What it adds:** Distinguishing walking, standing, sitting, climbing stairs, or falling — useful for both navigation context and safety (fall detection).

- **Approach:** IMU-based activity classification (lightweight model, runs easily on-device) rather than vision-based, for privacy and low compute cost.
- **Challenges:** False positives on fall detection; needs tuning per user.

## 8. Context-Aware Voice Assistant

**What it adds:** A voice assistant that already knows what the camera/sensors currently see, so the user doesn't have to describe context themselves.

- **Approach:** Voice command triggers a query against the current sensor-fusion state (last detected objects, distance readings, location) rather than a generic assistant call.

## 9. Adaptive Learning Based on User Behavior

**What it adds:** The system tunes alert sensitivity, vocabulary, and verbosity to the individual user over time (e.g., some users want terse alerts, others want detail).

- **Approach:** Simple on-device preference learning (adjusting thresholds/verbosity based on user feedback/corrections), not a full personalized model initially — keep it interpretable and user-controllable.

## 10. Indoor Localization

**What it adds:** GPS doesn't work indoors — indoor positioning (via BLE beacons, WiFi RTT, or visual-inertial odometry) fills that gap for buildings the user frequents.

- **Approach:** Start with BLE beacon triangulation in known/frequent locations (cheap, low-power) before attempting general-purpose visual-inertial indoor SLAM.

## 11. Digital Twin of Surroundings (Personal Scale)

**What it adds:** A lightweight, personal 3D/semantic model of frequently visited spaces (home, workplace) that the system can reference for navigation, updated incrementally rather than rebuilt from scratch each visit.

- **Approach:** Incrementally build and store a simplified point-cloud/semantic map per location using depth + detection data, refined over repeated visits.
- **Challenges:** Storage/versioning as environments change (furniture moves); this is a research-adjacent feature, not a near-term one — listed here because it's built from validated components (depth sensing + mapping), just not yet integrated.

## 12. Real-Time Hazard Prediction

**What it adds:** Predicting a likely collision course (e.g., a person or vehicle converging on the user's path) rather than only reacting once an object is within the ultrasonic range.

- **Approach:** Simple trajectory extrapolation from consecutive detection frames (bounding box position/size over time) to estimate closing speed and warn earlier for genuinely fast-approaching hazards.
- **Challenges:** Needs a low false-positive rate — over-alerting erodes user trust quickly.

## 13. Federated Learning (Longer-Term, Multi-User)

**What it adds:** Improve the shared detection/hazard models across many users' devices without centralizing raw camera data — relevant once/if this becomes a multi-user platform.

- **Note:** Only relevant once there's more than one active user; for a single-user personal project this is future-facing infrastructure, not a near-term task.

## 14. Emotion Recognition (Privacy-Preserving) — Use With Caution

**What it adds:** Could help a user know if a person they're speaking with looks upset or confused, as a social-context cue.

- **Approach, if pursued:** On-device only, no storage of facial data, opt-in and clearly disclosed to the visually impaired user; ideally the *other person's* face is not stored or transmitted anywhere.
- **Caution:** Emotion recognition from facial expression is scientifically contested in accuracy and cross-cultural validity, and raises real privacy/consent concerns for bystanders. This should be treated as a low-priority, carefully-scoped feature, not a core one.

---

## Summary Table

| Feature | Maturity today | Compute need | Priority for this project |
|---|---|---|---|
| VLM scene Q&A | Available (cloud/edge) | Medium–High | High |
| LLM assistant layer | Available | Low (cloud) / Medium (on-device) | High |
| Edge AI / on-device inference | Available | Medium | High (removes internet dependency) |
| Scene understanding / semantic mapping | Available, integration-heavy | Medium–High | Medium |
| Predictive navigation | Available (routing) + custom hazard logic | Low–Medium | Medium |
| Location memory | Straightforward | Low | Medium |
| Activity recognition (IMU) | Mature | Low | Medium |
| Context-aware voice assistant | Straightforward | Low | High |
| Adaptive personalization | Straightforward | Low | Medium |
| Indoor localization | Available (BLE/WiFi RTT) | Low–Medium | Medium |
| Digital twin (personal) | Emerging, integration-heavy | High | Low–Medium |
| Hazard prediction | Straightforward | Low | Medium |
| Federated learning | Mature technique, needs multi-user base | High infra | Low (future) |
| Emotion recognition | Contested accuracy, privacy-sensitive | Low | Low, opt-in only |
