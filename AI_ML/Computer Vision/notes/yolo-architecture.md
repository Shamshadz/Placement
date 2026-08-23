# YOLO architecture

> **Area:** Computer Vision  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-23  
> **Tags:** #yolo #object-detection #architecture

## In one minute

YOLO is a single-stage detector: one neural network maps an image directly to object locations and class scores. Although implementations vary by version, the reusable architecture is **backbone → neck → detection head**. The backbone extracts features, the neck combines them across scales, and the head predicts boxes and classes. This avoids a separate region-proposal stage, which is why YOLO is suited to real-time use.

## Architecture at a glance

    Input image
        ↓ resize / normalize / augmentation during training
    Backbone
        ↓ feature maps at several resolutions (for example P3, P4, P5)
    Neck: FPN / PAN multi-scale fusion
        ↓ fused feature maps for small, medium, and large objects
    Detection head
        ↓ box coordinates + objectness/class scores (and task-specific outputs)
    Post-processing
        ↓ confidence filtering + NMS, unless using an end-to-end NMS-free head
    Final detections

## Components

| Component | Role | Interview detail |
| --- | --- | --- |
| Input / preprocessing | Resizes and normalizes the image; training may add augmentation | Letterboxing preserves aspect ratio but requires mapping outputs back to the source image. |
| Backbone | Learns hierarchical visual features; shallow layers retain fine detail, deeper layers capture semantics | Examples across variants include Darknet-53 and CSP-based blocks such as C3 and C2f. |
| Neck | Fuses features across resolutions | FPN carries high-level semantics down to higher-resolution maps; PAN carries localization information upward. This is important for small objects. |
| Head | Converts fused features into location and class predictions at each scale | Historical YOLOv3/v5 heads are anchor-based; modern variants such as YOLOv8 use anchor-free, decoupled classification and box-regression branches. |
| Post-processing | Removes weak and duplicate predictions | NMS normally suppresses overlapping duplicate boxes; it can become a latency bottleneck in dense scenes. |

## Detection head: anchor-based versus anchor-free

Anchor-based heads predict offsets relative to predefined box shapes. They can require anchor selection/tuning when object-size distribution changes. Anchor-free heads predict relative to feature-map locations, eliminating anchor tuning. A decoupled head uses separate branches for classification and box regression, reducing conflict between the tasks. These are design choices of a specific implementation—not universal properties of every model called YOLO.

## Multi-scale detection

Objects occupy very different pixel sizes. A high-resolution map (for example, P3) preserves detail for small objects; lower-resolution maps (P4/P5) have larger receptive fields and are used for larger objects. The neck makes each prediction scale carry both local detail and global context.

## Training and inference

During training, predictions are assigned to ground-truth objects and optimized with classification and localization losses; variants differ in their exact objectness, IoU, and distribution-based regression losses. During inference, the model decodes predictions, filters by confidence, and removes duplicates. Evaluate the whole pipeline, including preprocessing and post-processing, because detector-only latency can hide the actual production cost.

## Version landmarks

| Family / generation | Useful architectural distinction |
| --- | --- |
| YOLOv1 | Framed detection as direct regression from full image to boxes and class probabilities. |
| YOLOv3 | Darknet-53 backbone, feature-pyramid multi-scale prediction, anchor-based head. |
| YOLOv5 | CSP-style C3 blocks, PAN-style path aggregation, and SPPF in common implementations; historically anchor-based. |
| YOLOv8 | C2f blocks and an anchor-free, decoupled head in Ultralytics’ implementation. |
| Later variants | May add attention, alternative heads, or NMS-free end-to-end inference; inspect the actual model/configuration instead of assuming behaviour from its name. |

## Trade-offs and pitfalls

| Choice / concern | Why it matters |
| --- | --- |
| Higher input resolution | Helps small objects, but increases compute, memory, and latency. |
| More pyramid levels | Broadens object-scale coverage, but costs compute and can complicate deployment. |
| NMS thresholds | Aggressive suppression may remove close true objects; weak suppression leaves duplicates. |
| Image resize policy | Stretching distorts objects; incorrect letterbox reversal misplaces boxes in source coordinates. |
| Architecture labels | YOLO versions come from different groups and repositories, so cite the exact implementation and checkpoint used. |

## Interview prompts

- Why does the neck need FPN/PAN-style multi-scale fusion when the backbone already creates feature maps?
- Compare anchor-based and anchor-free detection heads. What deployment or data issue could make the distinction important?
- Where would you look if small-object recall is poor but large-object recall is good?

## My example

Document the exact checkpoint, input size, which detection scales were used, confidence/NMS thresholds, hardware, end-to-end FPS, and per-class recall. For a tracker pipeline, also record how detector thresholds affected ID switches and missed tracks.

## References

- [Original YOLO paper](https://arxiv.org/abs/1506.02640)
- [Ultralytics YOLO architecture guide](https://docs.ultralytics.com/guides/yolo-architecture)
- [Feature Pyramid Networks paper](https://arxiv.org/abs/1612.03144)
