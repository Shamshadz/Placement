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
