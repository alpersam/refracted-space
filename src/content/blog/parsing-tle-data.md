---
title: "Parsing TLE Data with Python"
description: "A practical guide to reading Two-Line Element sets for satellite tracking, with code."
date: 2025-11-28
---

Two-Line Element sets (TLEs) are the lingua franca of satellite tracking. They're ugly, column-formatted, and encode everything you need to propagate an orbit — if you know where to look.

## The Format

A TLE looks like this:

```
ISS (ZARYA)
1 25544U 98067A   24001.50000000  .00016717  00000-0  10270-3 0  9993
2 25544  51.6400 208.9163 0006703 300.3030  59.7630 15.49560532999999
```

Line 1 carries the catalog number, classification, launch identifiers, epoch, and drag terms. Line 2 carries the classical orbital elements: inclination, RAAN, eccentricity, argument of perigee, mean anomaly, and mean motion.

## Parsing It

The key insight: TLEs are **fixed-width**, not delimited. You slice by column index, not by whitespace.

```python
def parse_tle(line1: str, line2: str) -> dict:
    return {
        "catalog_number": int(line1[2:7]),
        "epoch_year": int(line1[18:20]),
        "epoch_day": float(line1[20:32]),
        "inclination": float(line2[8:16]),
        "raan": float(line2[17:25]),
        "eccentricity": float(f"0.{line2[26:33]}"),
        "arg_perigee": float(line2[34:42]),
        "mean_anomaly": float(line2[43:51]),
        "mean_motion": float(line2[52:63]),
    }
```

The eccentricity field is the classic gotcha — it's stored as a decimal fraction with the leading `0.` assumed. Column 26 to 33 gives you `0006703`, which means `0.0006703`.

## Propagation

Once parsed, feed these elements into SGP4. The `sgp4` Python package handles this cleanly:

```python
from sgp4.api import Satrec, WGS72

satellite = Satrec.twoline2rv(line1, line2, WGS72)
```

From here you get position and velocity in TEME coordinates at any future (or past) epoch. The model is accurate to roughly a kilometer over a few days — good enough for pass predictions, not for collision avoidance.

## When TLEs Break

TLEs assume a simplified perturbation model. They degrade over time. For LEO objects, a TLE older than a week is suspect. For GEO, you have more margin, but the drag terms become meaningless.

Always check the epoch. Always fetch fresh data from [CelesTrak](https://celestrak.org) or [Space-Track](https://space-track.org). Never trust a cached TLE.
