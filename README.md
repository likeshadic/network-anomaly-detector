# Network Anomaly Detector

A custom-built network anomaly detection system running on a Raspberry Pi 5. It captures live network traffic, analyzes it using a TensorFlow Lite inference engine, and scores anomalies by severity in real time — providing lightweight, edge-based security monitoring for a home network.

---

## Overview

Most home networks have zero visibility into what's happening at the traffic level. This project changes that by running a dedicated anomaly detection system on a low-power device at the network edge.

The detector passively captures packets using `scapy`, extracts features from the traffic, and feeds them through a TensorFlow Lite model trained to identify anomalous patterns. Each detected anomaly is scored by severity, giving clear signal on what deserves attention versus what's routine noise.

---

## Architecture

```
Home Network Traffic
        │
        ▼
┌──────────────────────────┐
│   Raspberry Pi 5         │
│                          │
│   ┌──────────────────┐   │
│   │  Packet Capture  │   │
│   │  (scapy)         │   │
│   └────────┬─────────┘   │
│            │              │
│   ┌────────▼─────────┐   │
│   │ Feature Extraction│   │
│   └────────┬─────────┘   │
│            │              │
│   ┌────────▼─────────┐   │
│   │ TF Lite Inference │   │
│   │ Engine            │   │
│   └────────┬─────────┘   │
│            │              │
│   ┌────────▼─────────┐   │
│   │ Anomaly Scoring   │   │
│   │ & Logging         │   │
│   └──────────────────┘   │
│                          │
│   Runs as systemd service │
└──────────────────────────┘
```

---

## Features

- **Live Packet Capture** — Passively monitors network traffic using `scapy` without disrupting normal network operations
- **TensorFlow Lite Inference** — Lightweight ML model optimized for edge deployment on ARM hardware
- **Severity Scoring** — Each anomaly is assigned a severity score to help prioritize real threats over noise
- **Runs as a systemd Service** — Starts automatically on boot and runs continuously in the background
- **Low Resource Footprint** — Designed to coexist alongside other services (Pi-hole) on a Raspberry Pi 5

---

## Hardware

| Component | Detail |
|-----------|--------|
| Device | Raspberry Pi 5 |
| OS | Raspberry Pi OS (Debian-based, ARM64) |
| Also running | Containerized Pi-hole (DNS filtering) |
| Network position | Same LAN as Proxmox homelab server |

---

## Tech Stack

- **Python** — Core application language
- **TensorFlow Lite** — Inference engine for anomaly classification
- **scapy** — Packet capture and traffic analysis
- **systemd** — Service management and auto-start on boot

---

## How It Works

1. **Capture** — `scapy` listens on the network interface and captures packets in real time
2. **Extract** — Relevant features are extracted from each packet or batch of packets (protocol, size, timing patterns, etc.)
3. **Classify** — Features are passed through the TF Lite model, which determines whether the traffic is normal or anomalous
4. **Score** — Anomalous traffic is assigned a severity score based on the model's confidence and the nature of the anomaly
5. **Log** — Results are logged for review and analysis

---

## Current Status

- [x] Flow collector running as a systemd service, capturing and extracting 17 features per flow
- [x] Collection monitoring script for tracking progress and traffic patterns
- [x] Prometheus exporter script for exposing anomaly metrics
- [ ] Prometheus scrape configuration and Grafana dashboard integration
- [ ] Hailo AI accelerator integration for improved inference performance
- [ ] Expanded feature extraction for deeper traffic analysis
- [ ] Alerting via webhook or notification service for high-severity anomalies

---

## Related Projects

- [homelab-proxmox-server](https://github.com/likeshadic/homelab-proxmox-server) — Proxmox homelab running Grafana, Prometheus, and other services
- [homelab-project-pihole](https://github.com/likeshadic/homelab-project-pihole) — Containerized Pi-hole running on the same Raspberry Pi 5

---

## About

This project is part of my homelab environment and ongoing journey into DevOps and Cloud Engineering. It combines edge computing, machine learning, and network security — areas I'm passionate about exploring hands-on.

Built and maintained by [Daniel Campbell](https://github.com/likeshadic).
