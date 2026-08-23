# Computer vision annotations

> **Area:** Computer Vision  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-23  
> **Tags:** #data #annotation #yolo

## In one minute

Annotations are ground-truth labels that define what a model should learn and how it is evaluated. The annotation type must match the task: boxes for detection, masks for segmentation, keypoints for pose, and consistent object IDs across frames for multi-object tracking.

## Annotation types and quality

| Task | Label | Quality checks |
| --- | --- | --- |
| Detection | bounding box + class | tight boxes, correct class, include hard positives |
| Segmentation | polygon/mask + class | boundary accuracy and occlusion policy |
| Pose | keypoints + visibility | consistent skeleton order and visibility semantics |
| Tracking | per-frame box/mask + persistent ID | same object keeps its ID; handle entry/exit and occlusion consistently |

For Ultralytics YOLO detection, each image has an optional matching .txt file. Each object is one line: class_id x_center y_center width height, with zero-indexed class IDs and normalized xywh values in [0, 1]. The dataset YAML declares paths and class names. COCO commonly stores all annotations in JSON; convert carefully and verify class-ID mapping.

## Dataset workflow

1. Write a labeling guide: class definitions, box/mask boundaries, occlusion, truncation, ignored regions, and ambiguous cases.
2. Sample data across cameras, lighting, backgrounds, object sizes and rare cases.
3. Split by video, camera, location, or time—not random adjacent frames—to prevent leakage.
4. Audit labels, duplicates, class balance, empty images, and train/validation distribution.
5. Review errors after training and deliberately label the cases the model misses.

## Trade-offs and pitfalls

| Choice / concern | Why it matters |
| --- | --- |
| Random frame split | Near-duplicate frames can leak from training to validation and overstate quality. |
| Inconsistent occlusion policy | Produces noisy targets and unstable tracking. |
| Missing negatives | Causes false positives in deployment backgrounds. |
| Box-only labels | Cannot support pixel-accurate segmentation requirements. |

## Interview prompts

- How would you define annotation rules for partially occluded people?
- Why should video datasets be split by sequence rather than frame?

## References

- [Ultralytics detection dataset format](https://docs.ultralytics.com/datasets/detect/)
- [Ultralytics annotation guide](https://docs.ultralytics.com/guides/data-collection-and-annotation/)
