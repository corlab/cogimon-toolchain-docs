.. _development:

Developing with the Framework
=============================

Cloning the sample torque controller (mm-controller)
----------------------------------------------------

To clone and build the controller proceed as follows:

1. ``git clone https://github.com/corlab/rtt-gazebo-lwr-simulation.git mm-controller``

2. ``mkdir mm-controller/build && cd mm-controller/build``

3. Cmake command links to the volume:
 :code:`cmake -DOROCOS-RTT_DIR=/vol/cogimon/lib/cmake/orocos-rtt -DRCI_DIR=/vol/cogimon/share/rci0.5 -DRSC_DIR=/vol/cogimon/share/rsc0.13 -DNemoMath_DIR=/vol/cogimon/share/NemoMath0.5 -DRSB_DIR=/vol/cogimon/share/rsb0.13 ..`
4. ``make -j4``



Developing with the volume installation
---------------------------------------

Follow the steps as explained in :ref:`usage-preconfigured`. In order to use
your very own RTT component (e.g. a controller) with the volume installation,
you have to include an additional step between 2 and 3a:

After you have sourced the setup file with the line below,

    2.) setup up all necessary paths ``source etc/setupEnv.sh``

you have to add the path to the shared object of your RTT component to the
search path of orocos (e.g. mm-controller):

    2.1) e.g. ``export RTT_COMPONENT_PATH=/homes/.../mm-controller/build/src/orocos:$RTT_COMPONENT_PATH``


