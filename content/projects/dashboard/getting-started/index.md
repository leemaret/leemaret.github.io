---
title: "Getting started: Install, unit files, and configuration"
date: 2023-05-18T13:43:27-05:00
---

[Grafana](https://grafana.com/docs/grafana/latest) is the primary component of my dashboard and the main focus of the setup. Before jumping into the install, a few words about where this is being installed.

## self-hosted infrastructure

Cloud is cool but it is expensive and I am a systems administrator, so I have [a server](https://www.servethehome.com/hpe-proliant-microserver-gen10-plus-review-this-is-super/) on which I've installed [a hypervisor](https://www.proxmox.com/en/proxmox-ve). The server is plugged into a network appliance with multiple gigabit switches that runs a [pfsense](https://www.pfsense.org/) firewall.

With this setup I've created a network just for the server and all virtual machines running on it, and firewall rules so the network can only be accessed from outside by my primary dev computer on the home network. When new VMs or containers are created in proxmox, they grab a DHCP lease from the private network service running on pfsense. Pfsense is also running a certificate authority, a DNS resolver, and an ACME server within the network.

This setup is great - as long as I'm staying entirely within my home private network. In order to reach the network from outside my local, I'd have to set up a split-horizon DNS lookup on the network appliance and turn on a service to watch for public ip address changes on my home ip. I don't want to do that stuff because security, and also this is fine for my home use, so this constraint isn't a problem for me.

## dashboard environment overview

The general overview of the dashboard environment is:

- a virtual machine running Ubuntu, on which Grafana is installed;
- some containers managed by Proxmox within the network, running Alpine, on which random Go and Rust apps I've written are running;
- PostgreSQL instances running on the same host as Grafana;
- devices made from Feather and other non-Linux hardware with network capabilities, running Rust and C apps I've written.

The goal is to use Grafana to create dashboards and monitoring panels so I can take all these different data sources and turn them into a meaningful picture of how all my various applications and devices are doing at any given time. I'd also like to improve my understanding of common logging implementations for Rust and Go with standard log collection setups. This might require installing an additional log shipper (Prometheus) into the environment as I progress through the various data source setups. 

## installing Grafana

[Grafana docs page on installing.](https://grafana.com/docs/grafana/latest/setup-grafana)

### install from APT on Ubuntu

[Grafana docs page on installing from APT.](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/#install-from-apt-repository)

There is an interesting note on the install page regarding the OSS and the Enterprise versions. Two packages (OSS and Enterprise) are provided and two release branches (stable and beta). The docs note that Enterprise is the recommended and default version and is available for free. I think what this means in practical human language is that the Enterprise and OSS packages are the same except the Enterprise package supports plugins, one of which is probably the paid key to unlock the paid, actual enterprise features. I didn't actually confirm that with them or anything though, and I installed the OSS version, and it works fine. 

I found the instructions linked above worked with no issues and I was able to start the service in the next step without change from their provided instructions.

### why not install from the binary?

I installed Grafana first using the [binary install process](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/#install-grafana-using-a-deb-package-or-as-a-standalone-binary). This is actually a personal preference issue; some people have a good standard method for where they install binaries and how they handle upgrades, or more likely, they're making images to deploy to a pod or container as needed and use the binary as part of a helm chart or other automated install. I'm in neither situation here so when systemd couldn't start it after running `dpkg -i [binary_path]` I just uninstalled it and did it the easy way with APT.

## start the grafana-server service 

[Grafana docs page on starting the Grafana server.](https://grafana.com/docs/grafana/latest/setup-grafana/start-restart-grafana/)

The APT release includes a unit file, and the instructions linked above include standard systemctl start and control functions. The default unit file serves the application over port 3000. In my case, because I am deploying this to a virtual machine on my hypervisor in the private network, I added a DNS resolution host override record to Pfsense so I could reach the Grafana instance from my local without typing in the ip address. Because I installed to Ubuntu, port 3000 was already open. The linked instructions include how to alter the unit file if you wish to run on a port lower than 1024. 

When the Grafana service is successfully running and you can reach the host and port, you should be able to login at `host:3000` with the username and password `admin`. On login, you will be required to update the password before progressing.

## configure Grafana security

[Grafana docs on configuring security.](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/)

This is going to be an evolving section as I add data sources.

The page linked above has a number of default settings that should probably be changed depending on whatever one is trying to do. For example, users with the Viewer role can make any query to any data source in the organization, regardless of the queries defined in the panel. You probably don't want this! The linked documentation provides some examples for how to address. 

### about Grafana configuration

[Grafana docs on configuration.](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/)

Grafana uses .ini files and environment variables for configuration. The .ini file that is being used for the running instance is displayed by `systemctl status grafana-server.status`; on Linux the default is at `/etc/grafana/grafana.ini`.