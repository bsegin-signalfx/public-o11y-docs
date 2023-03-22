.. _mongodb-atlas-receiver:

***********************
MongoDB Atlas receiver
***********************

.. meta::
      :description: The MongoDB Atlas receiver allows the Splunk Distribution of OpenTelemetry Collector to collect metrics from MongoDB Atlas through its monitoring API.

The MongoDB Atlas receiver allows the Splunk Distribution of OpenTelemetry Collector to collect metrics from MongoDB Atlas through its monitoring API. The supported pipeline type is ``metrics``. See :ref:`otel-data-processing` for more information.

Database metrics are dimensionalized by project and database attributes, for example, ``project_name`` and
``database_name``.

.. note:: Use the MongoDB Atlas receiver in place of the SignalFx Smart Agent ``mongodb-atlas`` monitor type.

Get started
======================

Follow these steps to configure and activate the component:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform:
   
   - :ref:`otel-install-linux`
   - :ref:`otel-install-windows`
   - :ref:`otel-install-k8s`

2. Configure the MongoDB Atlas receiver as described in the next section.
3. Restart the Collector.

Sample configurations
----------------------

To activate the MongoDB Atlas receiver, add ``mongodbatlas`` to the ``receivers`` section of your
configuration file, as shown in the following example:

.. code:: yaml

   # In this example, both values are pulled from the environment.

   receivers:
     mongodbatlas:
       public_key: ${MONGODB_ATLAS_PUBLIC_KEY}
       # You can obtain the public key from the API Keys tab of the MongoDB Atlas Project Access Manager. 
       # This value is required.
       private_key: ${MONGODB_ATLAS_PRIVATE_KEY}
       # You can obtain the private key from the API Keys tab of the MongoDB Atlas Project Access Manager. 
       # This value is required.

To complete the configuration, include the receiver in the ``metrics`` pipeline of the ``service`` section of your
configuration file. For example:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [mongodbatlas]

Settings
======================

The following table shows the configuration options for the MongoDB Atlas:

.. raw:: html

   <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/receiver/mongodbatlas.yaml"></div>

Metrics
=====================

The following metrics, resource attributes, and attributes are available.

.. raw:: html

   <div class="metrics-component" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/metric-metadata/mongodbatlasreceiver.yaml"></div>

Deactivate metrics
--------------------------------

To deactivate any metric, apply the following configuration:

.. code:: yaml

   metrics:
     <metric_name>:
       enabled: false

Troubleshooting
======================

.. include:: /_includes/troubleshooting-components.rst