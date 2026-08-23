# Computer Vision — Revision Index

## Notes

- [Detection: YOLO model families and task types](notes/yolo-models.md)
- [Dataset annotation](notes/annotations.md)
- [Geometry: homography](notes/homography.md)
- [Tracking: Kalman filter and MOT algorithms](notes/multi-object-tracking.md)
- [Multi-view 3D tracking (MV3DT)](notes/mv3dt.md)

## Skills

### 1. Structure a revision note

1. Start with a one-minute answer: what it is, why it is used, and where it fits in a pipeline.
2. Explain the flow or mathematics with one concrete example.
3. Record failure modes and trade-offs; interviewers use these to test depth.
4. Add project-specific evidence: model, data, metric, latency/cost, and what you would improve.
5. End with two likely interview questions and a checkbox for whether you can explain it without notes.

Use the repository-wide [note template](../../docs/note-template.md). Keep each note narrow and link related notes from this index.

## Revision checklist

- [ ] I can distinguish detection, segmentation, pose estimation, and tracking.
- [ ] I can explain why tracking needs both motion and association.
- [ ] I can describe how calibration and homography relate image coordinates to a ground plane.
- [ ] I can defend the model/tracker choice for a real-time project.
