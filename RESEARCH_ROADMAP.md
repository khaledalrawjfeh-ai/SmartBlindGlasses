# Research Roadmap — Long-Horizon & Experimental Concepts

**Important framing:** Everything in this document is a research direction, not a product feature. Items here range from "exists commercially but not yet integrated" to "theoretical and decades out." Each entry states its Technology Readiness Level (TRL, 1–9 scale), current status, and known limitations. Nothing here should be read as a claim about what the current Smart AI Glasses prototype does or will imminently do.

A note on a specific idea worth addressing directly: the concept of sensors that "send images through the skull via waves/frequency directly into a blind person's brain to restore sight" is **not how any validated technology works today**. Non-invasive stimulation (e.g., transcranial magnetic or electrical stimulation) can influence neural activity in coarse, low-resolution ways, but it cannot transmit structured image data into the brain, and no non-invasive method currently produces functional vision. The closest real technologies (below) either require surgical implants with direct electrode contact to the retina, optic nerve, or visual cortex, or they use sensory substitution — converting images into sound or touch patterns the brain learns to interpret, without touching the brain electrically at all. These are described accurately below so the distinction is clear.

---

## 1. Visual Cortical / Retinal Prostheses (Invasive BCI for Vision)

- **TRL:** 7–9 for some systems (a few devices have reached limited commercial/clinical approval, e.g., historical retinal implants such as Argus II; several have since been discontinued for business reasons, not safety).
- **Status:** Clinically trialed, partially commercialized, still evolving rapidly (e.g., cortical implants like those from Second Sight/Cortigent successors and academic groups).
- **How it actually works:** Requires **surgical implantation** of electrode arrays directly on or in the retina, optic nerve, or visual cortex. An external camera feeds a processed, very low-resolution pattern of electrical stimulation to the implant. Output is described by users as flashes of light/patterns ("phosphenes"), not photographic vision.
- **Feasibility:** High cost, requires surgery and long rehabilitation, resolution is currently very limited (perceiving shapes/motion/large text at best, not detailed scenes).
- **Ethics/safety:** Surgical risk, long-term biocompatibility of implants, informed consent given the technology is still early and results vary a lot person to person, cost/access equity.
- **Relevance to this project:** Out of scope for a wearable, non-invasive glasses project — this is a separate, medical-device category requiring clinical partners. Included here only for completeness and to correctly frame what "restoring vision electronically" actually requires today.
- **Roadmap:** Not a target for this project. If ever pursued, would require partnership with a certified medical device team, not a hobbyist/personal engineering effort.

## 2. Non-Invasive Sensory Substitution (Vision-to-Sound / Vision-to-Touch)

- **TRL:** 6–8 — some systems are commercially available or in advanced trials (e.g., vOICe-style vision-to-sound, tongue-display and vibrotactile "BrainPort"-style devices).
- **Status:** Real, validated, non-invasive. Users learn to interpret patterned audio or tactile signals derived from camera images as spatial information over weeks to months of training.
- **How it works:** No brain stimulation at all — it's *learned perception*, similar to how someone learns to read Braille or interpret echolocation clicks. A camera feed is converted into a structured, consistent audio pattern (pitch = height, volume = brightness, pan = position) or a tactile grid (e.g., on the tongue or skin), and the brain's natural plasticity learns to extract spatial meaning from it.
- **Feasibility:** Buildable today with existing hardware (camera + DSP + audio or a vibrotactile array). This is the most realistic "beyond current features" direction for this project.
- **Ethics/safety:** Low risk (non-invasive), but requires user training time and has a real learning curve; expectations should be set honestly — it's a rich supplementary channel, not photographic vision.
- **Roadmap (0–3 years):** A genuinely buildable near-term research feature — encode ultrasonic + camera depth into a spatial audio or vibrotactile pattern as an *additional*, always-on channel alongside the existing spoken alerts.

## 3. Vibrotactile / Haptic Navigation Feedback

- **TRL:** 8–9 — mature and widely used (e.g., vibrating wearables for navigation, haptic canes).
- **Status:** Commercially available components (vibration motors, haptic driver ICs) make this straightforward to integrate.
- **Feasibility:** High — this is an incremental hardware addition, not a research risk.
- **Roadmap (0–1 years):** Add directional vibration motors (e.g., temple or wristband) to encode obstacle direction/urgency as a silent, low-latency complement to the buzzer — useful in loud environments or when the user prefers a discreet alert.

## 4. Auditory Spatial Perception (Spatial/3D Audio Cues)

- **TRL:** 8–9 — spatial audio libraries and HRTF-based rendering are mature and open-source.
- **Feasibility:** High — can run on existing compute; the main work is tuning HRTF profiles and latency.
- **Roadmap (0–1 years):** Render object-detection results as directional audio cues (a sound seeming to come from the object's actual direction) rather than only spoken text — faster to parse than a full sentence.

## 5. EEG-Based Intention / Attention Detection (Non-Invasive Neurofeedback)

- **TRL:** 4–6 depending on application — consumer EEG headsets exist (TRL 8-9 as hardware), but reliable *intention decoding* for practical control in daily wearable use is still largely research-stage, especially outside lab conditions.
- **Status:** Experimental for this use case. EEG signal quality from consumer-grade, non-gelled electrodes is noisy, and reliable classification typically needs controlled conditions or extensive per-user calibration.
- **Feasibility:** Motion artifacts (walking, talking) make EEG very difficult to use reliably in exactly the "walking around outdoors" scenario this project targets.
- **Ethics/safety:** Non-invasive EEG is low physical risk, but raises data-privacy questions (neural data is highly sensitive) and consent/interpretability concerns if decisions are made from noisy signals.
- **Roadmap (5–10+ years):** Worth monitoring as dry-electrode EEG and motion-robust decoding improve, but not realistic for near-term integration into an actively-moving wearable.

## 6. Emotion Recognition (Physiological, Privacy-Preserving)

- **TRL:** 6–7 for the underlying signal processing (heart-rate variability, skin conductance); lower for facial-expression-based approaches, which are scientifically contested.
- **Feasibility:** Simple biosensors (PPG, GSR) are cheap and available.
- **Ethics/safety:** Significant privacy and consent issues, especially since this device would also observe *other people's* faces/voices, not just the user's own state. Any physiological sensing should be about the wearer only, opt-in, and not persisted.
- **Roadmap:** Low priority; if pursued, restrict to the wearer's own physiological signals only (not inferring bystanders' emotions from their faces).

## 7. Sensor Fusion (Camera + LiDAR + Radar + IMU + Ultrasonic + Biosensors)

- **TRL:** 8–9 as individual mature technologies; 5–6 as an integrated wearable-scale fused system for this specific use case.
- **Status:** Each sensor type is mature and available in small/embeddable form factors (see `SENSOR_FUSION.md` for detail). The research/engineering work is in fusion — combining heterogeneous, noisy signals into one coherent, low-latency understanding on constrained wearable hardware.
- **Roadmap (1–3 years):** Realistic mid-term goal — this is a natural evolution of the current ultrasonic + camera setup and doesn't require any invasive or unproven neuroscience.

## 8. General Non-Invasive Brain Stimulation (TMS/tDCS-Class Methods) for Sensory Input

- **TRL:** 3–4 for the specific idea of "transmitting structured visual information" this way.
- **Status:** **Theoretical / not validated.** Techniques like transcranial magnetic or electrical stimulation can modulate broad regions of neural activity (used clinically for other purposes, e.g., some approved uses in neurology/psychiatry), but there is no established science showing they can deliver structured image data or restore functional vision non-invasively. Any claim to the contrary should be treated skeptically pending peer-reviewed evidence.
- **Ethics/safety:** Off-label or DIY brain stimulation carries real safety risk (skin burns, seizure risk in susceptible individuals, unknown long-term effects) and should never be pursued outside a supervised clinical/research setting.
- **Roadmap:** Not a near- or mid-term direction for this project. Included here explicitly so it's clearly separated from the validated sensory-substitution approaches in items 2–4, which achieve a similar *goal* (giving the brain spatial information about surroundings) through learned perception rather than direct neural signal injection.

---

## TRL Reference Scale (for the table above)

| TRL | Meaning |
|---|---|
| 1–2 | Basic principles observed / concept formulated |
| 3–4 | Proof of concept / lab validation |
| 5–6 | Prototype demonstrated in relevant environment |
| 7–8 | System demonstrated / qualified in operational environment |
| 9 | Actual system proven, commercially or clinically deployed |

## Summary: What's Actually Realistic Near-Term for *This* Project

1. Sensor fusion expansion (camera + depth/LiDAR + IMU + ultrasonic) — **buildable now**.
2. Non-invasive sensory substitution (vision-to-audio or vision-to-tactile) — **buildable now**, genuinely novel for this project, no unproven neuroscience required.
3. Vibrotactile directional feedback and spatial audio cues — **buildable now**, low risk, high value.
4. EEG-based interaction and non-invasive "brain input" — **research-watch only**, not realistic for a walking-around wearable in the near term.
5. Any form of direct neural image transmission or vision restoration via external waves/frequency — **not scientifically supported today**; the real analogous technologies are surgical retinal/cortical implants, a completely different (and separate, medical-device) engineering effort.
