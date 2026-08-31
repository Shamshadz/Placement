# Homography

> **Area:** Computer Vision  
> **Status:** reviewed  
> **Last reviewed:** 2026-08-23  
> **Tags:** #geometry #calibration #birdseye-view

## In one minute

A homography is a 3×3 projective transformation that maps points between two views of the same plane. In video analytics it commonly maps image pixels on a ground plane to a bird’s-eye-view or world-plane coordinate system, enabling distances, trajectories, zones, and speed estimates.

## How it works

For homogeneous image points x and x-prime, x-prime is proportional to Hx, where H is a 3×3 matrix with eight degrees of freedom (the overall scale is arbitrary). At least four non-collinear point correspondences on the same physical plane are needed to estimate H. In practice, use more correspondences with RANSAC to reject bad matches.

Pipeline: choose known ground-plane points → mark their image positions → estimate H → transform the bottom-centre of each detection box → do measurement/tracking in the transformed plane.

## Trade-offs and pitfalls

| Choice / concern | Why it matters |
| --- | --- |
| Planar assumption | A single homography is invalid for points not on the chosen plane or strongly non-flat terrain. |
| Correspondence quality | Poor clicks, moving markers, or lens distortion create large metric error. |
| Camera movement | Recompute calibration if the camera is bumped, zoomed, or moved. |
| Bottom-centre convention | It approximates the foot/ground contact point for people and vehicles; define it consistently. |

## Interview prompts

- Why are four point correspondences sufficient for a homography?
- Why can a homography estimate ground-plane speed but not arbitrary 3D distance?

## My example

Describe the scene plane, source of real-world coordinates, calibration validation error, and how you handled lens distortion and camera movement.

## References

- [OpenCV homography tutorial](https://docs.opencv.org/4.x/d1/de0/tutorial_py_feature_homography.html)

## Detailed calibration flow

~~~mermaid
flowchart LR
  A[Known ground-plane points] --> B[Mark corresponding image pixels]
  B --> C[Estimate H with RANSAC]
  C --> D[Validate with held-out points]
  D --> E[Transform detection foot points]
  E --> F[Birds-eye tracks, zones, speed]
~~~

Homography maps homogeneous coordinates. For an image point p=[u,v,1] transpose, compute q=Hp and divide the first two coordinates by q's third coordinate. That final division is what makes the transform projective rather than an ordinary affine matrix multiplication.

## Worked example

Mark corners of a rectangular floor area whose real-world coordinates are known in metres. Estimate H from at least four non-collinear pairs, then transform the bottom-centre of a person box in each frame. Compute speed from world-space displacement divided by time. Smooth the world-space track before reporting speed, otherwise detector jitter becomes false motion.

## Production considerations

- Correct significant lens distortion before estimating H; a planar transform cannot compensate arbitrary radial distortion.
- Store calibration version, camera ID, coordinate units, point pairs, reprojection error, and validation date.
- Raise an alert when fixed scene landmarks move in transformed coordinates; this can detect camera drift.
- Use a full calibrated 3D camera model when targets or measurements do not lie on one plane.

## Revision checklist

- [ ] I can explain why four non-collinear correspondences are the minimum.
- [ ] I can state why a ground-plane homography is invalid for arbitrary 3D points.
- [ ] I can describe how to validate and monitor a camera calibration.
