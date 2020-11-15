---
title: Filtering with Flux in InfluxDB
description: How does filtering works with Flux in InfluxDB?
categories: [InfluxDB, Flux, filtering, querying]
toc: false
layout: post
---
The `filter()` function behaves similarly to the `WHERE` command in SQL.

```flux
from(bucket: "trial_bucket")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "service_org_duration")
```

- The `fn` parameter expects a `predicate function`, that is an anonymous function producing a boolean (`true`/`false`) output. The filtering will include rows for which `fn` returns `true` based on the expression.
- The `r` argument of the `fn` parameter stands for the rows.
