.. _deployments-pivotal-cloudfoundry:

****************************
Pivotal Cloud Foundry 
****************************

.. meta::
      :description: Use Pivotal Cloud Foundry Tanzu to install and configure the OpenTelemetry Collector.

Use Pivotal Cloud Foundry (PCF) to deploy the Collector.

You have three deployment options:

* :ref:`otel-pcf-bosch`: The Collector is deployed to the PCF environment as a standalone deployment.

* :ref:`otel-pcf-buildpack`: This integration installs and runs the Collector as a sidecar to your app. In this configuration, the Collector runs in the same container as the app.

* :ref:`otel-pcf-tile`: The Tanzu Tile is a packaged release of the Collector that can be integrated into Pivotal Ops Manager. The Tanzu Tile allows you to download, install, run, configure, and update the Collector from the Ops Manager.

.. caution:: Only Tanzu Application Service (TAS) version 2 is supported. TAS version 3 is not supported.

.. _otel-pcf-bosch:

BOSH release
=========================

If deployed using the BOSH release, the Collector acts as a nozzle to :new-page:`Loggregator Firehose <https://docs.pivotal.io/tiledev/2-2/nozzle.html>`, which is one of the architectures Cloud Foundry uses to emit logs and metrics. 

Dependencies
----------------------------------

The release script requires:

* The :new-page:`BOSH CLI <https://bosh.io/docs/cli-v2-install/>``
* ``wget``
* ``jq``

Release and deploy the Collector
----------------------------------

Use the :new-page:`release script <https://github.com/signalfx/splunk-otel-collector/blob/main/deployments/cloudfoundry/bosh/release>` to generate a new release with the latest version of the Splunk Distribution of OpenTelemetry Collector. 

.. code-block:: 
   
   bosh -d splunk-otel-collector deploy deployment.yaml

See :new-page:`configuration examples <https://github.com/signalfx/splunk-otel-collector/tree/main/deployments/cloudfoundry/bosh/example>`.

Learn more
----------------------------------

See the following GitHub repos and files:

* The Collector's :new-page:`BOSH release repo <https://github.com/signalfx/splunk-otel-collector/tree/main/deployments/cloudfoundry/bosh>`.
* The Collector's :new-page:`development guide for PCF BOSH <https://github.com/signalfx/splunk-otel-collector/blob/main/deployments/cloudfoundry/bosh/DEVELOPMENT.md>`.

.. _otel-pcf-buildpack:

Cloud Foundry Buildpack
=========================

The Cloud Foundry Buildpack deploys the Collector as a sidecar for the actual app being deployed. The Collector is able to observe the app as a nozzle to the Loggregator Firehose, seeing all metrics and logs sent to the Loggregator Firehose as long as it's running.

Dependencies
----------------------------------

* ``wget``
* ``jq``

Install the pack and deploy the Collector
--------------------------------------------------

To install the Buildpack:

* Clone the Collector's :new-page:`Buildpack GitHub repository <https://github.com/signalfx/splunk-otel-collector/tree/main/deployments/cloudfoundry/buildpack>`.
* Go to the newly created repo.
* Run the following command to add the Buildpack for the Collector:

.. code-block:: 

   cf create-buildpack otel_collector_buildpack . 99 --enable

Learn more
----------------------------------

See the following GitHub repos and files:

* :new-page:`Configuration options <https://github.com/signalfx/splunk-otel-collector/tree/main/deployments/cloudfoundry/buildpack#configuration>`.
* :new-page:`Troubleshooting <https://github.com/signalfx/splunk-otel-collector/tree/main/deployments/cloudfoundry/buildpack#troubleshooting>`.

.. _otel-pcf-tile:

Tanzu Tile
=========================

The Tanzu tile uses the :ref:`otel-pcf-bosch` to deploy the Collector as a nozzle to :new-page:`Loggregator Firehose <https://docs.pivotal.io/tiledev/2-2/nozzle.html>`.

Dependencies
----------------------------------

The release script requires:

* The :new-page:`BOSH CLI <https://bosh.io/docs/cli-v2-install/>`
* The :new-page:`Tile generator <https://docs.vmware.com/en/Tile-Developer-Guide/2.10/tile-dev-guide/tile-generator.html>`
* ``wget``
* ``jq``

Release and deploy the Collector
----------------------------------

Run the following command to create the BOSH release packaged as a dependency of the Tanzu tile, and build the tile. 

.. code-block:: 

   ./make-latest-tile

If the command is successful, you can find the tile in ``./product/splunk-otel-collector-<VERSION>.pivotal``.

Learn more
----------------------------------

See the following GitHub repos and files:

* The Collector's :new-page:`Tanzu Tile GitHub repo <https://github.com/signalfx/splunk-otel-collector/tree/main/deployments/cloudfoundry/tile>`.