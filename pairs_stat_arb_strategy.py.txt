import pandas as pd
import datetime
import os

DATA_FILE = "data/sample_prices.csv"
LOG_FILE = "logs/monitor.log"

STALE_THRESHOLD = 60      # seconds
SPIKE_THRESHOLD = 0.003   # 0.3%


def log(msg):
    os.makedirs("logs", exist_ok=True)
    with open(LOG_FILE, "a") as f:
        f.write(f"{datetime.datetime.now()} | {msg}\n")


def run():
    df = pd.read_csv(DATA_FILE)
    df['timestamp'] = pd.to_datetime(df['timestamp'])

    # Missing
    missing = df[df['price'].isna()]

    # Stale
    df = df.sort_values('timestamp')
    df['time_diff'] = df['timestamp'].diff().dt.total_seconds()
    stale = df[df['time_diff'] > STALE_THRESHOLD]

    # Spike
    df['pct_change'] = df['price'].pct_change()
    spike = df[abs(df['pct_change']) > SPIKE_THRESHOLD]

    if not missing.empty:
        log(f"Missing prices: {len(missing)}")

    if not stale.empty:
        log(f"Stale ticks: {len(stale)}")

    if not spike.empty:
        log(f"Price spikes: {len(spike)}")

    if missing.empty and stale.empty and spike.empty:
        log("All checks passed")


if __name__ == "__main__":
    run()
