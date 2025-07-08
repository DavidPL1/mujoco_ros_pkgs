Troubleshooting
==================

This section provides solutions to common issues encountered when using MuJoCo ROS. If you have a problem that is not listed here, please consider opening an issue on the `GitHub repository`_.

.. _GitHub repository: https://github.com/ubi-agni/mujoco_ros_pkgs/issues

-----------------------------
General troubleshooting tips
-----------------------------

  * **Run the simulation with the option `verbose:=true`** to get more detailed output, which can help identify the source of the problem.

-----------------------------
Known Issues
-----------------------------
    * **MuJoCo version mismatch**: Ensure that the MuJoCo version configured in your environment matches the version used to build MuJoCo ROS. Mismatches lead to header/library incompatibilities.
    * **Python version mismatch**: Ensure that the Python version installed in your environment matches the version used to build MuJoCo ROS.
    * **Executing mujoco_ros.MujocoEnv.attach_viewer in python spawns a window that does not render content**: This has been experienced on systems with dedicated NVIDIA GPUs, where `prime-select` has been set to `on-demand`. Check by running `prime-select query`. If it is set to `on-demand`, try switching to `nvidia` by running `sudo prime-select nvidia` and rebooting your system. This should allow the viewer to render content correctly.

-------------
 Build issues
-------------
  * When using a tar archive
    * make sure it is compatible with the version of MuJoCo ROS you are using.  
    * make sure you are using the correct architecture for your system.
    * make sure `MUJOCO_DIR`, `LD_LIBRARY_PATH`, `CPATH`, and `LIBRARY_PATH` are set accordingly in your environment to include the MuJoCo installation directories.

  * With MuJoCo built from source
    * make sure you've checked out the correct tag compatible with the MuJoCo ROS version you are using.
    * make sure you've installed MuJoCo to your workspace's target install directory.