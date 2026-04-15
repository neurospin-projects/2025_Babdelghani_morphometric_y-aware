Global-to-Local Guidance for Cortical Sulcal Representation Learning
===================================================================

This repository contains the code for learning local cortical sulcal representations
with self-supervised learning while incorporating global morphometric guidance.
The project focuses on training and evaluating representation learning pipelines
for sulcal analysis using PyTorch-based models and contrastive learning tools.

This branch (``MiCCAI_brain_shape``) is dedicated to experiments related to
global brain-shape guidance for local sulcal representation learning.

Pipeline overview
-----------------

.. image:: figure_folder/pipeline.pdf
   :alt: Global-to-local cortical sulcal representation learning pipeline
   :align: center
   :width: 100%

The proposed framework combines a local sulcal encoder with global morphometric
information extracted from whole-brain descriptors. Global information can be
integrated in two main ways:

- **Y-aware weighted contrastive learning**: global morphometric information is used
  to weight relationships between samples and structure the latent space.
- **CLIP-style alignment**: local sulcal embeddings are aligned with embeddings
  derived from tabular global descriptors.

Repository structure
--------------------

- ``contrastive/``: training, evaluation, and experiment code
- ``AUTHORS.rst``: authorship information
- ``setup.py``: package installation and dependencies

Installation
------------

.. code-block:: bash

   git clone https://github.com/neurospin-projects/2025_Babdelghani_morphometric_y-aware.git
   cd 2025_Babdelghani_morphometric_y-aware
   git checkout MiCCAI_brain_shape

   python3 -m venv venv
   source venv/bin/activate
   pip install --upgrade pip
   pip install -e .

Requirements
------------

Main dependencies are defined in ``setup.py``.
Some scripts may additionally require a BrainVISA environment.

Training
--------

Training is controlled through the ``contrastive_model`` parameter in the configuration.

Train with Y-aware mode
~~~~~~~~~~~~~~~~~~~~~~~

Use this mode when global morphometric information is used to guide the contrastive
structure of the latent space through weighted similarities between subjects.

Set:

.. code-block:: yaml

   contrastive_model: YAware

Then launch training with your usual training command, for example:

.. code-block:: bash

   cd contrastive
   python3 train.py mode=encoder contrastive_model=YAware

Train with CLIP-style tabular alignment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use this mode when you want to align local sulcal embeddings with embeddings learned
from tabular global descriptors.

Set:

.. code-block:: yaml

   contrastive_model: CLIPTabular

Then launch training:

.. code-block:: bash

   cd contrastive
   python3 train.py mode=encoder contrastive_model=CLIPTabular

Practical guidance
~~~~~~~~~~~~~~~~~~

- Use ``contrastive_model: YAware`` when your objective is to inject global morphometric
  similarity directly into the contrastive loss.
- Use ``contrastive_model: CLIPTabular`` when your objective is to explicitly align
  sulcal representations with a tabular global-information encoder.
- Both modes rely on the same general training pipeline, but differ in the way
  global information is integrated into representation learning.

Evaluation
----------

.. code-block:: bash

   cd contrastive
   python3 evaluation/embeddings_pipeline.py

Data
----

This project relies on neuroimaging-derived sulcal data and some experiments
require UK Biobank-based pretraining data. Data access is not bundled in this repository.

