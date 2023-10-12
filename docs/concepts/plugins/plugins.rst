Plugins
=======

MuJoCo ROS implements a callback based plugin system targeted at allowing users to customize as much as possible. For this plugin systems we use pluginlib, which does not require the core application to be linked against the plugins, but instead allows us to load them at runtime.


A plugin's lifecycle consists of 6 stages:


**0. Discovery**:
Plugins are defined in the parameter server under `MujocoPlugins` (the server node first searches within its own namespace and then continues to search globally until the parameter name is matched). The `MujocoPlugins` parameter lists all plugins that should be loaded with a nested config, in case they need additional configuration.

An example config looks like this:

.. code-block:: yaml
  :caption: Example YAML configuration file

    MujocoPlugins:
    - type: plugin_package_name/SomePluginWithoutConfig
    - type: another_plugin_package/SomePluginWithConfig
        param0: "some string value"
        paramListofLists:
            [
                [0 0 0],
                [1 2 3],
                [2 2 2],
            ]
        NestedParamStruct:
            {
                structParam0: 10,
                structParam1: "some string",
                structParam2: True,
            }

On start, after having found where the plugin configuration resides on the parameter server, the simulation server ensures if the toplevel format is a list. If this step succeeds, the configuration is passed to the next stage of the cycle: the registration.


**1. Registration**:
In this phase, each element of the list, or each plugin, is fetched separately with its nested config, if it provides one. This configuration is then again checked to be in the correct format of a single name-value pair or a struct defining more nested elements.
This plugin config needs to provide at least a `type` which is used to identify which library to load.


**2. Instantiation**:
If the registration stage was successful, an instance of the plugin class is created and it receives its configuration, a reference to the server's ros node handle, and a reference to the environment it belongs to. If this process succeeds, the plugin instance is added to the list of instanciated plugins. Note that during initialization the plugin should not do anything yet.


**3. Load**:
Some time later when the server has completed the environment setup, the plugins are actually loaded. In this stage a plugin should complete its setup based on the information provided by its config, and the simulation model and data objects. If loading the plugin return with a success, the plugin instance is added to the list of callback-ready plugins.


**4. Run**:
Once the simulation has completed the setup phase, it starts the physics and rendering loops. The rendering loop performs simulation steps (or at least the forward part, in case it is paused). This triggers MuJoCo's internal calls to registered callbacks, which are set by the MuJoCo ROS server. The server then iterates through the callback-ready plugins (in the same order they were loaded) and calls the respective callback function in case it is overridden in the plugin.
Additional to the callbacks called by MuJoCo at the appropriate time in the physics step (currently only `control` and `passive`), we provide a `LastStageCallback`, which is called at the very end of a physics step.
The `RenderCallback`, on the other hand, is called from the render loop each time an image is being rendered by the GUI.
In case the plugin needs to react to a simulation reset (e.g. to reset status variables), a `reset` function can be overridden by the plugin, which will be called in case of a reset.


**5. Unload**:
When the simulation shuts down or performs a reload, the plugins are unloaded (before being loaded again in the latter case). During an unload the plugin has the chance to safely perform any necessary actions before the instance is destructed.


.. note::
    On a reload, the plugin config is fetched again from the parameter server. Changing the plugin configuration thus does not require a simulation restart, but only a reload.
