Structure-from-Motion Alignment Dataset
---------------------------------------

This archive contains a dataset for evaluating algorithms for aligning two
structure-from-motion reconstructions for which the relative pose and scale are unknown.

To construct the dataset, we recorded several image sequences in a controlled
indoor environment.  An ART-2 tracking system provided precise optical tracking
of a marker attached to the camera. One long image sequence was used in an offline,
incremental SfM pipeline to produce a point cloud reconstruction of the environment,
and to calibrate the transformation between the camera and the tracker coordinate systems.
We then recorded twelve image sequences in the tracked environment, and processed them in
a real-time capable keyframe-based SLAM system.

If you use this dataset, please cite the following paper:

Ventura, J., Arth, C., Reitmayr, G., & Schmalstieg, D. (2014). A minimal solution to the
generalized pose-and-scale problem. In Proceedings of the IEEE Conference on Computer
Vision and Pattern Recognition.

@inproceedings{ventura2014minimal,
  title={A minimal solution to the generalized pose-and-scale problem},
  author={Ventura, Jonathan and Arth, Clemens and Reitmayr, Gerhard and Schmalstieg, Dieter},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition},
  year={2014}
}

=============================================================================

Everything is stored as binary files with either float or double values.  The
extension tells you the format, e.g. double3 for 3-vectors of doubles.

In the top level are descriptors.float128 and points.double3, those are the 3D
points and their descriptors.

Each subfolder is one image sequence.  Each keyframe has descriptors, features
(2d points), a ground truth camera position, and the camera pose estimated by
SLAM (given as an SO3 rotation and a translation).  The file focalxy_centerxy
has the camera intrinsics (same for all).

Once you compute a similarity transform, you need to apply the transformation
given in coordstransform_rotation / coordstransform_translation to the corrected
camera center to evaluate the error w.r.t. the ground truth camera center:

error = ground_truth - coords_transform * corrected_slam_camera_center

This accounts for the transformation between the camera and the optical tracker
(this was estimated during the structure from motion bundle adjustment).

For a few keyframes, there was no tracker measurement, because I moved outside
of the tracking area (oops).  So, you have to ignore these when computing an
average error for each image sequence.

License
-------

This work is licensed under a Creative Commons Attribution 4.0 International License.

http://creativecommons.org/licenses/by/4.0/

