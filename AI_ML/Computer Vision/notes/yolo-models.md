# YOLO model families and task types

> **Area:** Computer Vision  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-23  
> **Tags:** #object-detection #yolo #real-time

## In one minute

YOLO (“You Only Look Once”) is a family of single-stage, real-time vision models. A model processes an image once and predicts locations plus classes directly, making it a common detector in video pipelines. “YOLO version” is not a strict standard: different research groups and implementations own different versions, so choose by the task, accuracy/latency target, hardware, tooling, and licence—not the largest version number.

## Model and task choices

| Need | Output | Notes |
| --- | --- | --- |
| Detection | class + bounding box | Best starting point for counting, alerts and MOT. |
| Instance segmentation | class + box + per-object mask | Use when object boundaries/area matter. |
| Pose | keypoints per instance | Use for posture, activity or ergonomics. |
| Classification | image-level class | Does not localize objects. |
| OBB | oriented bounding box | Useful for rotated objects such as aerial imagery. |

Model sizes commonly use a small-to-large scale (for example n, s, m, l, x in an implementation). Smaller models reduce latency and memory; larger models can improve accuracy but only help if the data, input resolution, and deployment budget support them.

## How it works

The usual pipeline is: resize/normalize image → backbone extracts features → neck combines multi-scale features → detection head predicts boxes, class scores, and task-specific outputs → confidence filtering and NMS (or an end-to-end alternative) produce final detections. Small objects benefit from adequate input resolution and multi-scale features, but that increases compute.

## Trade-offs and pitfalls

| Choice / concern | Why it matters |
| --- | --- |
| Detector vs tracker | A detector finds objects per frame; a tracker associates those detections across frames and assigns IDs. |
| Confidence threshold | Too high misses occluded/far objects; too low feeds false positives to the tracker. |
| mAP alone | It misses latency, per-class recall, calibration, and downstream ID stability. |
| Version labels | Do not imply that all YOLO releases share one architecture, codebase, licence, or training recipe. |

## Interview prompts

- Why choose a one-stage detector such as YOLO for a live camera pipeline?
- How would you select a detector for small, crowded objects on edge hardware?

## My example

For a video analytics project, state the chosen model/task, input size, device, precision (FP32/FP16/INT8), target FPS, measured end-to-end latency, and the validation metric that mattered most.

## References

- [Ultralytics model/task overview](https://docs.ultralytics.com/models/)
- [YOLO architecture review](https://arxiv.org/abs/2304.00501)
