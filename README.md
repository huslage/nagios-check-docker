nagios-check-docker
===================

This plugin checks that the docker daemon can be connected to and queried remotely. It also pulls in numeric metrics that are returned by the `docker info` command.

The plugin is designed to be forward-compatible. If the data returned by the API changes, command line options allow it to be configured to receive new metrics.

### Installation

Make sure you have a working Nagios instance.

