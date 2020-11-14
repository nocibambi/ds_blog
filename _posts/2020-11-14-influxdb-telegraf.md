---
title: Writing data to InfluxDB with Telegraf
description: How to use Telegraf to write data to InfluxDB?
categories: [InfluxDB, time-series, database]
toc: false
layout: post
---
## What is Telegraf?

[Telegraf](https://www.influxdata.com/time-series-platform/telegraf/) is InfluxData's time series data collection and reporting agent. Its main functionality is to collect data with the help of its plugins from a wide range of sources and to feed it to an InfluxDB database. It also can ingest data from InfluxDB databases and generate reports on them.

At the time of writing, it has more than 200 integrations, like Amazon ECS and Docker. You can see the full list [here](https://www.influxdata.com/products/integrations/).

Using Telegraf with InfluxDB most importantly consists of configuring Telegraf either automatically or manually. Configurations are stored in `telegraf.conf`.

The following steps assume that you have Telegraf installed and running. If this is not the case, you can follow the instructions [here](https://docs.influxdata.com/telegraf/v1.16/introduction/installation/).

## Automatic configuration

You can set up a subset of Telegraf plugins automatically (for the rest, do manual configuration).

Steps in the InfluxDB UI:

1. Open the **Data** screen from the navigation menu
2. Select the **Telegraf** tab
3. Select **Create Configuration**
4. In the **Bucket** dropdown, choose the bucket to where you want to write the data
5. Select the plugin groups you would like to use and click **Continue**. This will open a new window
6. In the window, add a name and description (optional)
7. If needed, configure the individual plugins within the group. (A green checkmark denotes plugins with no need of configuration)
8. Click **Create and Verify**. This opens a test page.
9. In the test page, follow the instructions to set up telegraf and click **Listen to Data**. This should result in a **Connection Found!** message.
10. You can close the screen with the **Finish** button.

For using Telegraf with Window host monitoring, you may need to do some following configuration, see [here](https://docs.influxdata.com/influxdb/v2.0/write-data/no-code/use-telegraf/auto-config/#windows).
