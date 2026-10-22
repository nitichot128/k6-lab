# K6 Performance Testing Lab

**Course:** ITDS362 — Software Quality Assurance and Testing

## Overview
This lab focuses on performance testing using Grafana K6 against the QuickPizza demo website.

## Part 0 — K6 Basics
- HTTP requests and response checks
- Virtual users and iterations
- API testing and authentication
- Performance thresholds
- Bug hunting

## Part A — Performance Testing
- A1: Smoke Test
- A2: Average Load Test
- A3: Stress Test
- A4: Spike Test
- A5: Soak Test

## Part B — Reflection
Analysis of performance testing results and answers to nine reflection questions.

## How to Run
Install Grafana K6 and run a test using:

```bash
k6 run a1-smoke.js
```

## Results
- `RESULTS.md` — Test results and reflection answers
- `reports/` — HTML performance reports for A2–A5

## Test Website
https://quickpizza.grafana.com