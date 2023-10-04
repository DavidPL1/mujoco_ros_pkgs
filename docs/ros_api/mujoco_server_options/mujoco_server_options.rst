Launchfile Arguments Explained
========================

use_sim_time
""""""""""""

This parameters decides whether MuJoCo ROS should publish its internal time as ROS time or if the ROS time is provided by another component.

If you are using an entirely simulated environment, you should set this parameter to ``true``. If instead you have real-world components such as robots in your environment you should set this parameter to ``false``.

Due to possibly severe consequences of accidentaly using simulated time in some scenarios this parameter is always explicitly required and does not employ a default.

ns
""""

This parameter can be supplied optionally to start the MuJoCo ROS server node in a specific ROS namespace.


console_config_file
"""""""""""""""""""

This parameter can be used to specify a custom ROS console file. By default it loads `this config <https://github.com/ubi-agni/mujoco_ros_pkgs/blob/noetic-devel/mujoco_ros/config/rosconsole.config>`_ which enables debug output for the mujoco_ros, mujoco_ros_control, and mujoco_ros_sensors namespaces.

**defaults to:** ``$(find mujoco_ros)/config/rosconsole.config``

.. attention:: This parameter only takes effect if ``verbose`` is enabled.

verbose
"""""""

``verbose`` toggles loading the configured ROS console configuration.

**defaults to:** ``false``

unpause
"""""""

Toggles whether the simulation should automatically start once it finished loading. In case you want to trigger the simulation to run from another ROS component or by the commandline, leave this parameter set to ``false``.

**defaults to:** ``false``


headless
""""""""

If you want to run MuJoCo ROS headless, i.e., without rendering the GUI, set this parameter to ``true``. Disabling the GUI might increase simulation speed, depending on your setup.

**defaults to:** ``false``

render_offscreen
""""""""""""""""

Cameras defined in the modelfile can be streamed out of the simulation over ROS using image transport. This requires offscreen rendering to be enabled. If this parameter is enabled, image transport and additional render and buffering resources will be allocated. If disabled, this step will be skipped upon loading.

**defaults to:** ``true``

.. hint:: Due to the cameras "on-demand" implementation, no offscreen rendering will take place if the image topics remain unsubscribed. Thus, if the topics remain unsubscribed, this will only reduce memory usage, but will have no impact on simulation speed.

no_x
""""

This parameter is a shorthand to set ``headless:=true`` and ``render_offscreen:=false``, and will always take precedence over both the other parameters. This parameter additionally disables initializing GLFW, which is a useful option on headless server which don't have an X server available.

**defaults to:** ``false``

admin_hash
""""""""""

Use this parameter to set a password for critical simulation operation like changing model properties over ROS service calls. This parameter only takes effect if ``eval_mode`` is enabled.

eval_mode
"""""""""

eval mode can be enabled to restrict access to certain features. For instance, when eval mode is disabled, the sensors plugin will create two topics for each sensor: one which adds configured noise and a second ground truth topic without noise. In eval mode, on the other hand, the ground truth data won't be published.
Also critical operations like pausing the simulation are only performed if the supplied admin_hash field in the request matches the configured hash.

**defaults to:** ``false``

.. attention:: This requires ``admin_hash`` to be set.

wait_for_xml
""""""""""""

If you want to fetch and load a model from the parameter server instead of providing a the path to a modelfile, you can enable this parameter.

**defaults to:** ``false``

realtime
""""""""

Set the desired realtime factor (between (0, 1]) to limit the simulation speed. E.g. ``.5`` will reduce simulation speed to 50% realtime. When this parameter is set to ``-1`` "unbound" mode is activated, i.e., the simulation will run as fast as possible. When left unset, the simulation defaults to a realtime factor of 1.

.. attention:: Interacting with the simulation through the GUI (pulling or rotating a body) in unbound execution mode might lead to much higher forces compared to bound execution mode.

num_sim_steps
"""""""""""""

To automatically shutdown the simulation after N simulation steps set this parameter to N. -1 disables this option.

**defaults to:** ``-1``

mujoco_plugin_config
""""""""""""""""""""

Provide the path to a yaml file to load a custom set of plugins.


modelfile
"""""""""

Provide the path to a MuJoCo modelfile to load it in MuJoCo ROS.

**defaults to:** ``$(find mujoco_ros)/assets/pendulum_world.xml``


initial_joint_states
""""""""""""""""""""

Provide the path to a yaml file containing joint positions and velocities to apply them on load or when the ``load_initial_joint_states`` service is called.

**defaults to:** ``$(find mujoco_ros)/config/initial_joint_states.yaml``


Developer Paramters
^^^^^^^^^^^^^^^^^^^

These parameters are interesting to Developers of MuJoCo ROS or its plugins and can probably be ignored by standard users.

debug
"""""

When enabled, this runs the server prefixed with ``gdb --args`` to debug with gdb.

**defaults to:** ``false``

debug_server
""""""""""""

When enabled this overrides the default debug mode starting gdb in the terminal you launch the launchfile from, and instead starts a gdb server at ``localhost:1234``.

**defaults to:** ``false``

valgrind
""""""""

If enabled, launches the server prefixed with ``valgrind $(valgrind_args)``

**defaults to:** ``false``

.. attention:: This mode is not combinable with ``debug:=true``.

valgrind_args
"""""""""""""

Set this parameter to supply additional valgrind arguments.

profile
"""""""

Save profiling output to ``/tmp/profile.out``.

**defaults to:** ``false``
