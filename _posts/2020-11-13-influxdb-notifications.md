---
toc: false
layout: post
description: How to create and manage notifications in InfluxDB?
categories: [InfluxDB, monitoring, time-series, notification]
title: Notifications in InfluxDB
---
## Notification endpoints

Notification endpoints define connections to third party services.

### Create notification endpoints

1. Select the **Alerts** screen from the navigation menu;
2. At the top of the **Notification Endpoints** section click the **Create** button;
3. Select a **Destination** from the drop-down menu;
4. Add a name and a description (optional);
5. Add the destination-specific configurations;
6. Click **Create**.

For specific details for the different endpoints, see the [documentation](https://docs.influxdata.com/influxdb/v2.0/monitor-alert/notification-endpoints/create/).

### Manage notification endpoints

In the **Alerts** menu, under the **Notification Endpoints** section, you can manage notifications. In particular, you can:

- Review the **list** of the existing notification endpoints;
- Review their **history** (eye icon);
- **Update** a notification behavior (by clicking on their name);
- **Rename** them (pen icon next to their name);
- **Turn them on/off** (toggle button);
- Add **labels** to them;
- **Delete** them (trash icon).

## Notification rules

Notification rules generate notifications by triggering notificaton endpoints based on a given condition regarding the outcome of checks.

### Create notification rules

Before your first notification rule, you need to have at least one notification endpoint in place.

1. Select the **Alerts** screen from the navigation menu.
2. At the top of the **Notification Rules** section click the **Create** button;
3. In the **About** section add
   1. **Name**
   2. **Schedule**: how often you would like the notification to review checks and trigger a message
   3. **Offset**: the delay relative to the start time of the intervals set under **Schedule**.
4. In the **Conditions** section, define the rule that will trigger the notification based on check statuses and (optionally) on tag values.
5. In the **Messages** section select which endpoint should the rule trigger.
6. Click **Create Notification Rule**.

### Manage notification rules

In the **Alerts** screen, under the **Notification Rules** section, you can manage notifications. In particular, you can:

- Review the **list** of the existing notification rules;
- Review their **history** (eye icon, **View History**);
- Create a **clone** of an existing notification (clone icon)
- **Update** a notification behavior (by clicking on their name);
- **Rename** them (pen icon next to their name);
- **Turn them on/off** (toggle button);
- Add **labels** to them;
- **Delete** them (trash icon).
