## nagios-check-docker

This plugin checks that the docker daemon can be connected to and queried remotely. It also pulls in numeric metrics that are returned by the `docker info` command.

The plugin is designed to be forward-compatible. If the data returned by the API changes, command line options allow it to be configured to receive new metrics. It has been tested with Docker 0.8 and 0.9 on both Debian Wheezy and Ubuntu 13.10.

### Requirements
* Python
* Nagios 3+
* docker-py
* nagiosplugin

### Installation

Make sure you have a working Nagios instance. This plugin can be run remotely via NRPE or locally.

Clone the repository, install the required pip dependencies and copy the script into the nagios plugin directory:

```
git clone https://github.com/huslage/nagios-check-docker.git
cd nagios-check-docker
sudo pip install -r requirements.txt
sudo cp check_docker /usr/local/bin/check_docker
```

#### Nagios Configuration

If you are using the local socket interface (default), you need to add the nagios user to the docker group:

`sudo usermod -aG docker nagios`

Then add the following to your nagios config:

```
define command{
    command_name    check_docker
    command_line    /usr/local/bin/check_docker
}
```

If you want to use the http interface, simply add `--url http://127.0.0.1:4243` to the end of the command_line. Feel free to use the plugin remotely by using the URL of the host where docker is running.

#### Plugin Options

The plugin automatically checks that the docker service is reachable and gathers these performance metrics:

* Containers
* Debug
* IPv4Forwarding
* Images
* MemoryLimit
* NEventsListener
* NFd
* NGoroutines
* SwapLimit

The default thresholds are 0 for both Critical and Warning, so they will not cause an alarm.

You can add arbitrary new metrics to be gathered by the plugin that are returned by the `docker info` command. Simply add a `-m <metric name> <warning> <critical>` command line option to the command_line in Nagios' config. This also allows you to override the default metric thresholds. 

For instance:

```
define command{
    command_name    check_docker
    command_line    /usr/local/bin/check_docker -m Containers 150 200 
}
```
