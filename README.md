# Mobile Application I/O Traces

This repository contains block-level I/O traces obtained from several widely used mobile applications, as referenced in the research paper [*Space-efficient FTL for Mobile Storage via Tiny Neural Nets*](https://dl.acm.org/doi/abs/10.1145/3688351.3689157).

## Overview

The I/O traces were collected on a Pixel 6a mobile device by instrumenting the Android Linux kernel with a custom eBPF probe. The probe captures all I/O operations at the block layer, ensuring Host Performance Booster (HPB) functionality was disabled throughout the collection process. These traces capture real-world usage patterns, including background I/O activity, although over 95% of the recorded I/O operations were attributed to the specific applications under study.

### Applications Covered
The trace dataset includes I/O operations from the following applications:

- **Mobile Games**: Pubg, Call of Duty, Diablo Immortal, Genshin Impact
- **Social Media**: Telegram
- **Image Gallery Slideshow**: F-Stop Gallery
- **Video Editor**: YouCut Video Editor

Traces were gathered during active interaction with each application for an extended period, reflecting realistic workload characteristics. Background I/O from other processes running on the phone is also present, providing a holistic view of system-level storage access.

### Fragmentation and SSD Usage
Prior to trace collection, the SSD on the Pixel 6a had been heavily utilized, leading to a high degree of fragmentation in the Logical Page Number (LPN) space. This fragmentation is evidenced by the large span of accessed LPNs observed across the 120GB SSD, contributing to the realistic representation of storage access patterns.

## Trace Parts: Installation and Execution

Each trace is divided into two distinct phases, and each phase is stored in a separate CSV file:

1. **Installation I/O (`<trace_name>_precond.csv`)**: This file contains the write I/O that occurs during the installation of the application on the phone. It is used to precondition the Flash Translation Layer (FTL) by initializing the Logical-to-Physical (L2P) mappings. 
   
2. **Execution I/O (`<trace_name>_exec.csv`)**: This file captures the I/O operations generated during the actual execution and active usage of the application.


## Trace File Format

The traces are available in CSV format, with the following fields:

| Column      | Description                                                            |
|-------------|------------------------------------------------------------------------|
| `process`   | The name of the process issuing the I/O request                        |
| `device`    | The block storage device being accessed                                |
| `rw_flag`   | Indicates the operation type: 'R' for read, 'W' for write              |
| `sector`    | The starting sector of the I/O operation (in units of 512-byte sectors)|
| `size`      | The size of the I/O operation (in units of 512-byte sectors)           |
| `timestamp` | The time of the I/O operation (in seconds since epoch)                 |
