
Getting Started
***************

System requirements
===================

Tosca has been tested on OSX and Ubuntu.

``git`` must be installed on your system, as well as ``gradle``.

Download
========

Tosca is hosted on github.com. To download and enter the directory, type the following commands in a terminal:

.. code-block:: sh

    git clone https://github.com/tosca-lang/tosca.git
    cd tosca

The build system is Gradle, and typing this command will build everything:

.. code-block:: sh

    ./gradlew

To make sure that everything is working as planned, just type ``./gradlew smoketest`` and you should see a bunch of tests running successfully.

How to continue
===============

For the manual, jump to the :doc:`intro` .
