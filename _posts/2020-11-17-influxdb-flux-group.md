---
title: Grouping data with Flux in InfluxDB
description: How can you group data with Flux in InfluxDB?
categories: [InfluxDB, Flux, time-series]
toc: false
layout: post
---

Grouping happens based on **group keys** which effectilve are the name of columns used for the grouping. A group then consists of rows having identical values in the group key columns.

## Example

Let's consider the http api request duration data.

```flux
from(bucket: "trial_bucket")
  |> range(start: -15m)
  |> filter(fn: (r) => r._measurement == "http_api_request_duration_seconds")
```

In respect to the time columns, it is 'grouped', by default, by its 'start' and 'end' times.

![default grouping](/images/influxdb/influxdb-group-default-data.png) "Default grouping"

![default chart](/images/influxdb/influxdb-group-default-chart.png) "Default grouping as chart"

## Grouping

Now, with the following snippet, we group data along the `_measurement` and `_field` columns.

```flux
from(bucket: "trial_bucket")
  |> range(start: -15m)
  |> filter(fn: (r) => r._measurement == "http_api_request_duration_seconds")
  |> group(columns: ["_measurement", "_field"])
```

This will change the group keys:

![grouped keys](/images/influxdb/influxdb-group-time-data.png)

And will also shape the charts:

![grouped by time chart](/images/influxdb/influxdb-group-time-chart.png)

As observations of the same time now belong to the same data, plotting them as a line chart on the time axis produces vertical lines.

## Aggregate and ungroup

For the next step let's calculate the mean values of the request durations.

```flux
from(bucket: "trial_bucket")
  |> range(start: -15m)
  |> filter(fn: (r) => r._measurement == "http_api_request_duration_seconds")
  |> group(columns: ["_time"])
  |> mean()
  |> group()  
```

While we calucalted the mean values for each timestamp, this also resulted in separate tables consisting of a single value. We can use the `group()` function to ungroup data, that is, to merge all tables into a single table.

This will generate the following chart.

![mean data](/images/influxdb/influxdb-group-degroup-chart.png)

We can produce the same result by explicitly turning off grouping on specific columns with the `mode` parameter.

## `mode`

The `group()` function also accepts a `mode` parameter with the following values:

- `by`: Group by the columns defined in the `columns` parameter.
- `except`: Group by the columns **not defined** in the `columns` parameter.

Accordingly, we can modify the above code to get the same result:

```flux
from(bucket: "trial_bucket")
  |> range(start: -15m)
  |> filter(fn: (r) => r._measurement == "http_api_request_duration_seconds")
  |> group(columns: ["_time"])
  |> mean()
  |> group(columns: ["_time", "_value"], mode: "except")
```
