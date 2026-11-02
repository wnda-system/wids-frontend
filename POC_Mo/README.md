# POC_Mo — dashboard pages from the proof of concept (Mo)

The two pages the sensor served during the proof-of-concept phase (Aug–Oct
2026), plain HTML + vanilla JS served by Flask on the sensor itself. They are
not part of this React app; they are here as working references for the
wireframes (overview, alerts, device status) and for which numbers proved
useful in practice.

The pages are copies — the live versions are in the edge repository
(`POC_Mo/mo_app/templates/`). They call the sensor's Flask API, so they don't
run on their own.

## Screenshots

| Screenshot | Page |
|---|---|
| [`monitor_dashboard.png`](screenshots/monitor_dashboard.png) | `/monitor` — the whole live dashboard |
| [`monitor_training_data.png`](screenshots/monitor_training_data.png) | `/monitor` — the Training data card |
| [`capture_field_profile.png`](screenshots/capture_field_profile.png) | `/` — capture page, Field test profile with the ground-truth panel |

## `/monitor` (`pages/monitor.html`)

| Card | What it shows | Data (sensor API) |
|---|---|---|
| Sensor | interface, channel/width, start/stop, seed baseline, replay a recording | `POST /api/monitor/start`, `/stop`, `GET /api/monitor/baselines`, `POST /api/monitor/import` |
| Brain Power | readiness ring (0–100) from week coverage, threshold stability and false alarms/hour; model A trusted or learning | `GET /api/monitor/status` → `brain` |
| Training data | which days the detectors train on, excluded periods, pause learning, data retention, preview of coverage | `GET/POST /api/monitor/training`, `POST /api/monitor/training/pause` |
| KPIs | frames/s, data frames/s, % encrypted, % retries, average signal, transmitters/min, alerts in the last hour | `status.kpi` |
| Traffic, last 10 minutes | stacked data / management / control frames per second, deauth marked | `status.series` |
| Anomaly score (model A) | score per second against the alert threshold | `status.series.a_score`, `status.detector.a_threshold` |
| Alerts | time, length, detector (A/B/A+B), likely type, confidence, evidence | `status.alerts` |
| Alerts by type, 24 h | count per type split by detector A / B / A+B | `status.alerts_by_type_24h` |
| New APs waiting for approval | BSSIDs announcing a protected SSID that aren't in the baseline; approve one or all | `status.pending_aps`, `POST /api/monitor/approve_ap` |
| Access points on this channel | SSID, BSSID, signal, beacons/s, protected, in baseline | `status.aps` |
| Baseline coverage by hour | 7 × 24 heat map of data collected per hour of the week | `status.brain.coverage_grid` |
| Model C (shadow mode) | connection, latency, model version, per-class thresholds, shadow alerts in 24 h | `status.model_c` |

The page polls `GET /api/monitor/status` every 2 s; the WNDA dashboard will use
SSE for alerts instead.

## `/` capture page (`pages/index.html`)

Two profiles: **Default** (free settings, CSV download) and **Field test
(AWID3)** (fixed settings for field experiments, plus a ground-truth panel:
attack type, PMF on/off, notes, "Attack started" / "Attack ended", add a
period afterwards). Calls `POST /capture`, `GET /status/<id>`,
`POST /stop/<id>`, `POST /capture/<id>/ground_truth`, `GET /download/<id>`.

## Worth carrying over

- One readiness number (Brain Power) with its three parts explained was the
  quickest way to tell whether alerts can be trusted yet.
- Alerts need the detector, a confidence level and the evidence (top
  deviating features) next to the type, or they can't be judged.
- Splitting alert counts by detector showed at a glance which detector was
  noisy.
- A device-status page should show what broke most often on the POC: the
  adapter dropping out of monitor mode, no protected network heard on the
  channel (DFS), the remote model unreachable, disk use.
