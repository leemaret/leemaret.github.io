---
title: "Overview of the dashboard project"
date: 2023-05-08T14:56:31-05:00
---

_Dashboard_ is one of the most abused words in tech. It is almost meaningless without a lot of context and understanding about the data being viewed and what it is being viewed for. It could be quarterly business reporting, or sales metrics, or ad hoc reporting from a live data source like a fleet of vehicles on a map. 

Usually in the contexts I care about (as the admin of some computers and devices), a dashboard refers to time series data being presented in a graphical format that can be queried and re-presented on the fly using common reporting and data visualization tools to take a closer or higher-level view of all the data. It's for keeping an eye on your system(s) and seeing how they are responding to various issues over time, as opposed to reporting on customers or business functions. 

## Project goals

I have a hypervisor on which I run several virtual machines and containers. Each instance has a number of applications, some installed and some I've created myself, from which I'd like to capture some metrics as they run. With these metrics I'd like to build a dashboard or several I can serve from a web instance available within my network, so I can click a link and see how all my stuff is doing with a nice visual interface.

## Not goals

I'm not interested in reinventing any wheels; I'd like to use off the shelf open source components for everything. Additionally, I'd like everything to be able to be self-hosted and not reliant on a cloud service, even if it is free.

## Components

The most critical component of a dashboard is actually the log forwarder. The reason why is if you can't reliably get your metrics out of your application or host or wherever into your time series db where it can be queried, you are going to suffer. So I need to do some more refining of my goals and testing to figure out both what I'm forwarding and how easy it is to add metrics endpoints from a Go/Rust/Kotlin app or a Linux service on a host and into the log forwarder.

### data sources

For the type of dashboard I am building, the data source is probably not going to be the data used by the application(s) you're monitoring. Usually you want metrics from the application itself, which may or may not have their own database separate from the application. If not, no biggie, it's all going to another database before querying anyway. In this case I know I'll want to capture performance data for a postgreSQL instance. I'd also like to add metric endpoints in any applications I write and test them against this system. 

### time-series database

Data sources are aggregated into a _time-series database_ which is a database optimized for data coming in regularly over time and queried against a time range. I have at least two choices:

- Grafana's built in TSDB
- InfluxDB

### log forwarder

Logs are shipped from various data sources by the _log forwarder_ to the time-series database. Usually the size of a particular operational dashboard is limited by the capabilities and speed of their log forwarder and configuration related to how much log data they keep around to query (_log retention_) and how frequently the window of time available logs expire (_log rotation_) before they are moved to a cheaper database.

I don't expect to have huge data needs even over time, so I'm optimizing for ease of setup and adding new metrics hooks into my applications. 

- Telegraf (influxdb) or Prometheus

### graphical visualization and query

Dashboards require visualization and I like Grafana. It has a lot of nice built-in tools for querying the data and building dashboards (like composable panels and a query language that can be used in the web url). Grafana also has some cool features to get me to my desired end state with the dashboard, like a concept of a _playlist_ where a list of dashboards can be displayed in a sequence.

- Grafana
