.. _usage:

Using the Framework
===================

.. _usage-preconfigured:

Using the pre-configured volume installation
--------------------------------------------

In this case everything is already configured properly. No installation is
necessary. Even some convenience functionalities are provided.

1. goto the correct volume ``cd /vol/cogimon``
2. setup up all necessary paths ``source etc/setupEnv.sh``
3. do steps 1.-2. for 3.a, 3.b and 3.c.
    a. start the Gazebo **server** with the specific plugins and a desired world-file
     :code:`./bin/gzserver -s /vol/cogimon/lib/orocos/gnulinux/rtt_gazebo_system/librtt_gazebo_system.so /vol/cogimon/etc/worlds/scenario1.world`
    b. start the Gazebo **client**
     :code:`./bin/gzclient`
    c. load the LWR 4 robot model into Gazebo
     :code:`./bin/rsb-toolscl0.13 call 'socket:/GazeboDeployerWorldPlugin/spawnModel("/vol/cogimon/etc/lwr-robot-description/lwr-robot.urdf")'`

4. load the components necessary to simulate the KUKA
 :code:`./bin/rsb-toolscl0.13 call -l /vol/cogimon/share/rst0.13/proto/sandbox/rst/cogimon/ModelComponentConfig.proto 'socket:/GazeboDeployerWorldPlugin/deployRTTComponentWithModel(pb:.rst.cogimon.ModelComponentConfig:{component_name:"lwr_gazebo" component_type:"LWRGazeboComponent" component_package:"rtt-gazebo-lwr-integration" model_name:"kuka-lwr" script:"/vol/cogimon/etc/worlds/scenario1.ops"})'`
5. send joint angles to the template controller
 :code:`./bin/rsb-toolscl0.13 send -I/vol/cogimon/share/rst0.13/proto/stable -l/vol/cogimon/share/rst0.13/proto/stable/rst/kinematics/JointAngles.proto 'pb:.rst.kinematics.JointAngles:{angles: [0,-0.8,0,0.4,0,0,0]}' 'socket:/rtt/lwr/cmd/position'`

If you want to use your own controller see :ref:`development`.

Troubleshooting
"""""""""""""""

.. important:: If you want to stop/kill all the components, be sure to kill the **gzclient** before the **gzserver**!
.. important:: Check your rsb.conf, if you run in problems with converters etc...

Make sure to have a properly configured **rsb.conf**. A simple working file can be found here:

.. code-block:: bash
    :emphasize-lines: 2,23,28

    [transport.socket]
    enabled = 1    # ENABLE to use socket transport!
    # SETUP the correct mapping between types!
    converter.cpp.".rst.dynamics.Pressures" = rst::dynamics::Pressures
    converter.cpp.".rst.geometry.Lengths" = rst::geometry::Lengths
    converter.cpp.".rst.geometry.Pose" = rci::Pose
    converter.cpp.".rst.signalprocessing.InstantaneousPhase" = rst::signalprocessing::InstantaneousPhase
    converter.cpp.".rst.cbse.ComponentState" = cca::ComponentState
    converter.cpp.".rst.dynamics.Forces" = rci::Forces
    converter.cpp.".rst.dynamics.JointImpedance" = rci::JointImpedance
    converter.cpp.".rst.signalprocessing.InstantaneousPhase" = rst::signalprocessing::InstantaneousPhase
    converter.cpp.".rst.cbse.ComponentState" = cca::ComponentState
    converter.cpp.".rst.dynamics.JointTorques" = rci::JointTorques
    converter.cpp.".rst.dynamics.Wrench" = rci::Wrench
    converter.cpp.".rst.geometry.Translation" = rci::Translation
    converter.cpp.".rst.kinematics.JointAccelerations" = rci::JointAccelerations
    converter.cpp.".rst.kinematics.JointAngles" = rci::JointAngles
    converter.cpp.".rst.kinematics.JointVelocities" = rci::JointVelocities
    converter.cpp.".rst.math.VectorDouble" = rci::Doubles
    converter.cpp.".rst.cbse.Tick" = cca::timing::Tick

    [transport.spread]
    enabled = 0    # DISABLE because we are using the socket transport!
    host = localhost
    port = 4803

    [plugins.cpp]    # LOAD the required plugins!
    load = rsbspread:rsbintrospection:rsbrstconverterssandbox:rsbrstconvertersrci:rsbrstconvertersstable

    [introspection]
    enabled = 1    # enable to make use of introspection tools (optional).

General Usage
-------------

Lorem Ipsum