Using the Framework
===================

Using the pre-configured volume installation
--------------------------------------------

In this case everything is already configured properly. No installation is
necessary. Even some convenience functionalities are provided.

1. goto the currect volume
 :code:`cd /vol/cogimon`
2. setup up all necessary paths
 :code:`source etc/setupEnv.sh`
3. do steps 1.-2. for 4.a, 4.b, 4.c and 4.d.
    a. start the Spread transport
     :code:`./sbin/spread -n localhost`
    b. start the Gazebo **server** with the specific plugins and a desired world-file
     :code:`./bin/gzserver -s /vol/cogimon/lib/orocos/gnulinux/rtt_gazebo_system/librtt_gazebo_system.so /vol/cogimon/etc/worlds/scenario1.world`
    c. start the Gazebo **client**
     :code:`./bin/gzclient`
    d. load the LWR 4 robot model into Gazebo
     :code:`./bin/rsb-toolscl0.13 call 'spread:/GazeboDeployerWorldPlugin/spawnModel("/vol/cogimon/etc/lwr-robot-description/lwr-robot.urdf")'`

4. load the components necessary to simulate the KUKA
 :code:`./bin/rsb-toolscl0.13 call -l /vol/cogimon/share/rst0.13/proto/sandbox/rst/cogimon/ModelComponentConfig.proto 'spread:/GazeboDeployerWorldPlugin/deployRTTComponentWithModel(pb:.rst.cogimon.ModelComponentConfig:{component_name:"lwr_gazebo" component_type:"LWRGazeboComponent" component_package:"rtt-gazebo-lwr-integration" model_name:"kuka-lwr" script:"/vol/cogimon/etc/worlds/scenario1.ops"})'`
5. send joint angles to the template controller
 :code:`./bin/rsb-toolscl0.13 send -I/vol/cogimon/share/rst0.13/proto/stable -l/vol/cogimon/share/rst0.13/proto/stable/rst/kinematics/JointAngles.proto 'pb:.rst.kinematics.JointAngles:{angles: [0,-0.8,0,0.4,0,0,0]}' 'spread:/rtt/lwr/cmd/position'`


**TODO** *change all paths to /vol/cogimon*


Troubleshooting
"""""""""""""""

.. important:: If you want to stop/kill all the components, be sure to kill the **gzclient** before the **gzserver**!
.. important:: Check your rsb.conf, if you run in problems with converters etc...

Make sure to have a properly configured **rsb.conf**. A simple working file can be found here:

.. raw:: html

   <link href='https://fonts.googleapis.com/css?family=Droid+Sans+Mono' rel='stylesheet' type='text/css'>
   <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.0/jquery.min.js"></script>
   <link href="_static/specials-board.css" type="text/css" rel="stylesheet" />
   <script src="https://cdn.rawgit.com/google/code-prettify/master/loader/prettify.js"></script>
   <body onload="">
   <script type="text/javascript">
      $(document).ready(function(){
         prettyPrint()
         $('.prettyprint').each(function(i) {
            var classList = $(this).attr("class").split(" ");
            // iterate through list
            for (var iter = 0; i < classList.length; iter++) {
               if (classList[iter].startsWith("highlight-")) {
                  // split line numbers to highlight
                  var lineNumbers = classList[iter].replace("highlight-", "").split("-");
                  // outsource into another function TODO
                  var olContainer = $(this).find("ol.linenums");
                  var allLIs = $(olContainer).find("li");


                  console.log(lineNumbers);
                  for (var liIter = 0; liIter < allLIs.length; liIter++) {
                     for (var numIter = 0; numIter < lineNumbers.length; numIter++) {
                        //var tmp = allLIs[liIter].className;
                        //tmp = tmp.replace("L","");

                        if (liIter == lineNumbers[numIter]) {
                           $(allLIs[liIter]).addClass('selectedLine');
                           break;
                        }
                     }
                  }
                  break;
               } else {
                  continue;
               }
            }


         });
      });
   </script>
   <pre class="prettyprint linenums highlight-1-4-27">
   [transport.socket]
   enabled = 0    # DISABLE because we are using spread!

   [transport.spread]
   enabled = 1    # ENABLE to use spread transport!
   host = localhost
   port = 4803
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

   [plugins.cpp]    # LOAD the required plugins!
   load = rsbspread:rsbintrospection:rsbrstconverterssandbox:rsbrstconvertersrci:rsbrstconvertersstable

   [introspection]
   enabled = 1    # enable to make use of introspection tools (optional).
   </pre>


General Usage
-------------

Lorem Ipsum