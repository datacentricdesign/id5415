---
layout: minimal
title: "Documentation"
permalink: /documentation/index.html
description: "Structure of the master branch at the end of the course."
---

Here is a snapshot of what the minimal documentation should include by the end of the course.

`S1` refers to Step 1 while `T1.1` refers to Task 1.1 as part of assignments and lab experiments

```
./id5415-repository                     
├── CHANGELOG.md                        # High-level report of each iteration
├── LICENSE
├── README.md                           # What is this repo about, who are the contributors,
│                                       # how is the source code organised, pic of how the prototype looks
├── docs
│   ├── README.md                       # Any explanation about the documentation structure
│   ├── alternative_architectures.md    # Assignment 4, S1 Network architecture
│   ├── analytics.ipynb                 # Lab XP 6
│   ├── api_specification.yaml          # Lab XP 5, S1 API Specification
│   ├── images                          # All images of the doc
│   └── xp
│       ├── lab-xp-1.md                 # T2.1 IoT Stacks, T3.4 Logs, T4.3 Grafana Visualisation
│       ├── lab-xp-2.md                 # T1.3 CSV data collection, T3.3 Grafana dashboard
│       ├── lab-xp-3.md                 # T1.1 refactor, T1.2 clean, T1.3 make a service,
│                                       #       T2.2 Architecture, T3.3 events Step 4 Flow chart
│       ├── lab-xp-4.md                 # S3 NetworkScanner Integration
│       ├── lab-xp-5.md                 # S2 MQTT Architecture diagram
│       └── lab-xp-6.md                 # S2 Design process dashboard, T3.1 Data inventory, T3.2 Data inventory 
│
└── src                                 # Directory containing your commented code
```