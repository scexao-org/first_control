.. first_control documentation master file, created by
   sphinx-quickstart on Mon Aug 26 15:50:58 2024.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to FIRST-PL's documentation!
=====================================

**Contact:** Sébastien Vievard (vievard@naoj.org)

This documentation is organized around the main observing workflow:

* learn what FIRST-PL does and how the instrument is arranged;
* prepare the SCExAO environment and start the control processes;
* acquire, align, and track a target;
* save and reduce the resulting data.

.. toctree::
   :maxdepth: 2
   :caption: FIRST-PL
   
   intro
   instrument_principle
   observing_procedure
   instrument_performance

.. toctree::
   :maxdepth: 2
   :caption: Setting up FIRST-PL

   instrument_setup

.. toctree::
   :maxdepth: 2
   :caption: Operate FIRST-PL
   
   quick_start
   command_reference
   operation_mainscript
   operation_acquiring
   operation_eon

.. toctree::
   :maxdepth: 2
   :caption: Work with data
   
   saving_images
   image_reconstruction
   pipeline_intro
   pipeline_recipes
   pipeline_utility_tools

.. toctree::
   :maxdepth: 2
   :caption: Miscellaneous
   
   troubleshooting
   misc

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
