Quickstart MuJoCo Server
========================

Launchfile Arguments Explained
------------------------------

use_sim_time
""""""""""""

This parameters decides whether MuJoCo ROS should publish its internal time as ROS time or if the ROS time is provided by another component.

If you are using an entirely simulated environment, you should set this parameter to ``true``. If instead you have real-world components such as robots in your environment you should set this parameter to ``false``.

Due to possibly severe consequences of accidentaly using simulated time in some scenarios this parameter is always explicitly required and does not employ a default.

ns
""""

This parameter can be supplied optionally to start the MuJoCo ROS server node in a specific ROS namespace.


console_config_file (= $(find mujoco_ros)/config/rosconsole.config)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This parameter can be used to specify a custom ROS console file. By default it load `this config<https://github.com/ubi-agni/mujoco_ros_pkgs/blob/noetic-devel/mujoco_ros/config/rosconsole.config>`_ which enables debug output for the mujoco_ros, mujoco_ros_control, and mujoco_ros_sensors namespaces.

.. note:: This parameter only takes effect if ``verbose`` is enabled.

verbose (= false)
"""""""""""""""""

``verbose`` toggles loading the configured ROS console configuration.

unpause (= false)
"""""""""""""""""

Toggles whether the simulation should automatically start once it finished loading. In case you want to trigger the simulation to run from another ROS component or by the commandline, leave this parameter set to ``false``.

headless (= false)
""""""""""""""""""

If you want to run MuJoCo ROS headless, i.e., without rendering the GUI, set this parameter to ``true``. Disabling the GUI might increase simulation speed, depending on your setup.

render_offscreen (= true)
"""""""""""""""""""""""""

Cameras defined in the modelfile can be streamed out of the simulation over ROS using image transport. This requires offscreen rendering to be enabled. If this parameter is enabled, image transport and additional render and buffering resources will be allocated. If disabled, this step will be skipped upon loading.

.. note:: Due to the cameras "on-demand" implementation, no offscreen rendering will take place if the image topics remain unsubscribed. Thus, if the topics remain unsubscribed, this will only reduce memory usage, but will have no impact on simulation speed.

no_x (= false)
""""""""""""""

This parameter is a shorthand to set ``headless:=true`` and ``render_offscreen:=false``, and will always take precedence over both the other parameters. This parameter additionally disables initializing GLFW, which is a useful option on headless server which don't have an X server available.

admin_hash (= '')
"""""""""""""""""

Use this parameter to set a password for critical simulation operation like changing model properties over ROS service calls. This parameter only takes effect if ``eval_mode`` is enabled. 

eval_mode (= false)
"""""""""""""""""""

When 

wait_for_xml (= false)
""""""""""""""""""""""

If you want to fetch and load a model from the parameter server instead of providing a the path to a modelfile, you can enable this parameter.

realtime (= "")
"""""""""""""""

Set the desired realtime factor (between (0, 1]) to limit the simulation speed. E.g. ``.5`` will reduce simulation speed to 50% realtime. When this parameter is set to ``-1`` "unbound" mode is activated, i.e., the simulation will run as fast as possible. When left unset, the simulation defaults to a realtime factor of 1.

.. note:: Interacting with the simulation through the GUI (pulling or rotating a body) in unbound execution mode might lead to much higher forces compared to bound execution mode.  

num_sim_steps (= -1)
""""""""""""""""""""

To automatically shutdown the simulation after N simulation steps set this parameter to N. -1 disables this option.

mujoco_plugin_config (= "")
"""""""""""""""""""""""""""

Provide the path to a yaml file to load a custom set of plugins.


modelfile (= $(find mujoco_ros)/assets/pendulum_world.xml)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provide the path to a MuJoCo modelfile to load it in MuJoCo ROS.


initial_joint_states (= $(find mujoco_ros)/config/initial_joint_states.yaml)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provide the path to a yaml file containing joint positions and velocities to apply them on load or when the ``load_initial_joint_states`` service is called.


Developer Paramters
^^^^^^^^^^^^^^^^^^^

These parameters are interesting to Developers of MuJoCo ROS or its plugins and can probably be ignored by standard users. 

debug (= false)
"""""""""""""""

When enabled, this runs the server prefixed with ``gdb --args`` to debug with gdb.

debug_server (= false)
""""""""""""""""""""""

When enabled this overrides the default debug mode starting gdb in the terminal you launch the launchfile from, and instead starts a gdb server at ``localhost:1234``.

valgrind (= false)
""""""""""""""""""

If enabled, launches the server prefixed with ``valgrind $(valgrind_args)``

.. note:: This mode is not combinable with ``debug:=true``.

valgrind_args = (= "")
""""""""""""""""""""""

Set this parameter to supply additional valgrind arguments.

profile (= true)
""""""""""""""""

Save profiling output to ``/tmp/profile.out``.
