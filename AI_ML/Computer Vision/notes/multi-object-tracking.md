# Kalman filter and multi-object tracking algorithms

> **Area:** Computer Vision  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-23  
> **Tags:** #tracking #kalman-filter #mot #re-identification

## In one minute

Multi-object tracking (MOT) turns frame-level detections into trajectories with stable IDs. A tracking-by-detection pipeline predicts each track’s next state, matches detections to predictions, updates matched tracks, and manages new/lost tracks. The Kalman filter supplies cheap motion prediction; association can use geometry, appearance, and camera-motion compensation.

## Kalman filter

The state contains a box representation and its velocity (for example centre, aspect ratio, height, and their velocities). The filter alternates between:

1. Predict: use a motion model to estimate the next state and increase uncertainty.
2. Update: combine the prediction with a detection measurement, weighted by process noise Q and measurement noise R.

It is efficient and works well for approximately linear, short-term motion with Gaussian uncertainty. It does not itself decide which detection belongs to which target; the association stage does.

## Tracker comparison

| Tracker | Main association idea | Strong point | Main limitation |
| --- | --- | --- | --- |
| ByteTrack | Match high-confidence detections first, then associate low-confidence detections with remaining tracks | Recovers occluded objects without a separate ReID model | Quality depends strongly on detector scores and motion/IoU matching |
| BoT-SORT | Kalman motion + IoU; adds camera-motion compensation and optional ReID appearance | Robust general-purpose tracker, especially with moving cameras | ReID and compensation add compute/tuning |
| OC-SORT | Observation-centric correction of Kalman motion during occlusion | Strong when motion is non-linear or occlusions are longer | Still relies on detector quality; no appearance signal by default |
| StrongSORT | Strengthened DeepSORT: improved detections/embeddings/association; StrongSORT++ adds AFLink and GSI | Appearance-aware identity continuity and offline-style enhancements | More compute and model dependencies than IoU-only trackers |

## Association and lifecycle

First gate impossible pairs using motion uncertainty or IoU, then compute a cost from IoU, Mahalanobis distance, and optionally ReID embedding distance. Solve one-to-one matching (often Hungarian assignment). Confirm new tracks only after enough evidence; retain lost tracks briefly to bridge misses; delete after a timeout. Tune these thresholds jointly with detector confidence and NMS.

## Metrics

MOTA mixes false positives, misses, and identity switches; IDF1 emphasizes identity correctness; HOTA balances detection and association. Report latency/FPS and failure cases alongside a metric.

## Trade-offs and pitfalls

| Choice / concern | Why it matters |
| --- | --- |
| IoU-only tracking | Fast, but IDs switch when similar objects cross or occlude each other. |
| Appearance / ReID | Improves re-association but can fail on uniforms, lighting changes, and domain shift. |
| Low-confidence detections | Can recover occluded targets, but can also attach false positives. |
| Fixed linear motion | Prediction drifts through long occlusion or abrupt turns. |

## Interview prompts

- What does a Kalman filter contribute to SORT-style tracking, and what does it not solve?
- When would you prefer ByteTrack over StrongSORT?

## References

- [ByteTrack paper](https://arxiv.org/abs/2110.06864)
- [BoT-SORT paper](https://arxiv.org/abs/2206.14651)
- [OC-SORT paper](https://arxiv.org/abs/2203.14360)
- [StrongSORT paper](https://arxiv.org/abs/2202.13514)

## Detailed tracking lifecycle

~~~mermaid
flowchart LR
  A[Detector boxes] --> B[Predict each Kalman state]
  B --> C[Gate candidate pairs]
  C --> D[Cost: motion, IoU, appearance]
  D --> E[One-to-one assignment]
  E --> F[Update matched tracks]
  E --> G[Age unmatched tracks]
  A --> H[Create candidate tracks]
  H --> I[Confirm after evidence]
  F --> J[Emit stable IDs]
  I --> J
~~~

The Kalman filter maintains a state mean and uncertainty. Prediction advances the state using a motion model and increases uncertainty. Update combines a matched detection with the prediction; an uncertain prediction should accept a wider candidate region than a confident one. Hungarian assignment chooses a one-to-one minimum-cost set after impossible pairs are gated out.

## Worked association example

At frame t, a track predicts a person box near x=200. Two detector boxes appear: one with high IoU and matching appearance, another nearby with a different embedding. Geometry and appearance together should choose the first. During an occlusion, ByteTrack can use a lower-confidence detection to keep the trajectory alive; StrongSORT may rely more on appearance; OC-SORT corrects motion using observations after the occlusion.

## Production considerations

- Tune detector confidence, maximum lost age, match threshold, and ReID threshold jointly on representative videos.
- Log ID switches, track fragmentation, unmatched detections, time since update, and per-camera failures.
- Benchmark HOTA/IDF1 alongside MOTA, throughput, and latency; MOTA can hide identity errors.
- Protect against a sudden detector outage by exposing track confidence and avoiding unsafe downstream actions based on extrapolated tracks alone.

## Revision checklist

- [ ] I can distinguish prediction, gating, assignment, update, confirmation, and deletion.
- [ ] I can explain why a Kalman filter does not solve identity association by itself.
- [ ] I can choose a tracker based on camera motion, occlusion, ReID cost, and latency.
