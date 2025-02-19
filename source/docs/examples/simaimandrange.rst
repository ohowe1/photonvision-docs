Simulating Aiming and Getting in Range
======================================

The following example comes from the PhotonLib example repository (`Java <https://github.com/PhotonVision/photonvision/tree/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-java-examples/src/main/java/org/photonlib/examples/simaimandrange>`_/`C++ <https://github.com/PhotonVision/photonvision/tree/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-cpp-examples/src/main/cpp/examples/simaimandrange>`_). Full code is available at those links.


Knowledge and Equipment Needed
-----------------------------------------------

- Everything required in :ref:`Combining Aiming and Getting in Range <docs/examples/aimandrange:Knowledge and Equipment Needed>`.

Background
----------

The previous examples show how to run PhotonVision on a real robot, with a physical robot drivetrain moving around and interacting with the software.

This example builds upon that, adding support for simulating robot motion and incorporating that motion into a :code:`SimVisionSystem`. This allows you to test control algorithms on your development computer, without requiring access to a real robot.

.. raw:: html

        <video width="85%" controls>
            <source src="../../_static/assets/simaimandrange.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

Walkthrough
-----------

First, in the main :code:`Robot` source file, we add support to periodically update a new simulation-specific object. This logic only gets used while running in simulation:

.. tab-set-code::

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-java-examples/src/main/java/org/photonlib/examples/simaimandrange/Robot.java
      :language: java
      :lines: 118-128
      :linenos:
      :lineno-start: 118

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-cpp-examples/src/main/cpp/examples/simaimandrange/cpp/Robot.cpp
      :language: c++
      :lines: 72
      :linenos:
      :lineno-start: 72

Then, we add in the implementation of our new `DrivetrainSim` class. Please reference the `WPILib documentation on physics simulation <https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-simulation/physics-sim.html>`_.

Simulated Vision support is added with the following steps:

Creating the Simulated Vision System
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

First, we create a new :code:`SimVisionSystem` to represent our camera and coprocessor running PhotonVision.

.. tab-set-code::

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-java-examples/src/main/java/org/photonlib/examples/simaimandrange/sim/DrivetrainSim.java
      :language: java
      :lines: 72-93
      :linenos:
      :lineno-start: 72

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-cpp-examples/src/main/cpp/examples/simaimandrange/include/DrivetrainSim.h
      :language: c++
      :lines: 78-92
      :linenos:
      :lineno-start: 78


Next, we create objects to represent the physical location and size of the vision targets we are calibrated to detect. This example models the down-field high goal vision target from the 2020 and 2021 games.

.. tab-set-code::

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-java-examples/src/main/java/org/photonlib/examples/simaimandrange/sim/DrivetrainSim.java
      :language: java
      :lines: 95-108
      :linenos:
      :lineno-start: 95

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-cpp-examples/src/main/cpp/examples/simaimandrange/include/DrivetrainSim.h
      :language: c++
      :lines: 94-109
      :linenos:
      :lineno-start: 94

Finally, we add our target to the simulated vision system.

.. tab-set-code::

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-java-examples/src/main/java/org/photonlib/examples/simaimandrange/sim/DrivetrainSim.java
      :language: java
      :lines: 113-114
      :linenos:
      :lineno-start: 113

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-cpp-examples/src/main/cpp/examples/simaimandrange/include/DrivetrainSim.h
      :language: c++
      :lines: 45-49
      :linenos:
      :lineno-start: 45

If you have additional targets you want to detect, you can add them in the same way as the first one.


Updating the Simulated Vision System
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once we have all the properties of our simulated vision system defined, the work to do at runtime becomes very minimal. Simply pass in the robot's pose periodically to the simulated vision system.

.. tab-set-code::

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-java-examples/src/main/java/org/photonlib/examples/simaimandrange/sim/DrivetrainSim.java
      :language: java
      :lines: 122-140
      :linenos:
      :lineno-start: 122

    .. rli:: https://raw.githubusercontent.com/PhotonVision/photonvision/661f8b2c0495474015f6ea9a89d65f9788436a05/photonlib-cpp-examples/src/main/cpp/examples/simaimandrange/cpp/sim/DrivetrainSim.cpp
      :language: c++
      :lines: 31-50
      :linenos:
      :lineno-start: 31

The rest is done behind the scenes.
