# Market Data Pipeline Monitoring & Alerting System

This project simulates a lightweight monitoring system for market data feeds used in trading environments.

## Overview
The system performs basic validation checks on incoming price data to identify common data quality issues that can impact trading decisions.

## Features
- Detects missing price values
- Identifies stale ticks based on time gaps
- Flags abnormal price movements (spikes)
- Logs issues for monitoring and debugging

## Tech
- Python (pandas)
- Bash (script execution)
- Linux cron (scheduled runs)

## Scheduling
```bash
*/5 * * * * /bin/bash /path/to/run_monitor.sh
