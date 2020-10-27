---
toc: false
layout: post
description: The outline of the introductory InfluxDB ebook I plan to write.
categories: [influxDB, gumroad, ebook, content strategy]
title: InfluxDB ebook outline 
---
I am taking part of the [Gumroad 14 day product challenge](https://gumroad.com/l/BLwER) where I decided to write an introductory ebook for influxDB as it currently has the biggest chance to succeed in my current workflow.

## Audience

My target audience is data scientists who would like to start to use InfluxDB. They are not total beginners as they have an idea of what time series and databases are. Still, they have no experience with time series databases as such and zero experience with influxDB.

## Content strategy

I am really behind this. I identified the following platforms where I could promote the process:

### Social media channels

- Twitter
- LinkedIn
- Facebook (especially groups)
- Instagram

### Blogging sites

- Medium (esp data science publications)
- Dev.to
- Hackernoon
- KDnuggets
- Dzone

### Quora

### Link sharing sites

- Hackerrank
- Reddit (subreddits)

### Social aggregators

I am also trying to find a social media aggregator in this other post. Any suggestions are wellcome!

## Outline

Title: InfluxDB for beginners

### Introduction

- What are time-series databases
- How InfluxDB compares to others
- The outline of this book

### Installation and setup

- Download
- Install
- Configuration
- Getting the Influx version

### Functional overview

- The overview of InfluxDB and its place within the influxdata ecosystem (Tick stack)
- InfluxDB OSS vs. Cloud
- Main functionalities
- Workflow

### Data format overview

- InfluxDB time-series format
- Tags and fields

### Loading/storing data

- Loading sample data
- Scraping data
- Storage size
- Data export/import
- How to format time
- Single vs. multiple measurements
- value limits
- Creating a database

### Querying

- Discussing flux its functional data scripting language
- The query language: select, where, group by, into, joins
- Math operations: count, min, max, arithmetic
- functions
- retention policy and continuous query

### Common issues (based on Stackoverflow)

(Maybe I remix this into the normal chapters)

- getting first/last values
- select where value is null
- How to delete data
- How to connect with grafana

### Use case study

An example case study also perhaps using other features of the ecosystem.

### Resources

- Where to learn more
- Learning strategies
- Ideas for implementation

### Optional (if I will have time)

Discussing the python client
