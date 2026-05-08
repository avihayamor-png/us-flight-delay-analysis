---
license: mit
task_categories:
- tabular-regression
language:
- en
tags:
- aviation
- flights
- delays
- machine-learning
size_categories:
- n<1K
---

![Banner](https://huggingface.co/datasets/avihayamor/US_flight-delay-2024/resolve/main/banner.png)

# U.S. Flight Delay Dataset — 2024

A hypothesis-driven exploratory analysis of 100,000 U.S. domestic flights from 2024, examining what actually drives arrival delays across airlines, airports, and times of day.

## Methodology

The analysis is structured around four research questions chosen before looking at the data, each targeting a different hypothesis about delay drivers (carrier, time-of-day, geography, and operational metrics). Findings are framed in terms of effect size and predictive strength, not just descriptive averages, so the dataset can serve as a starting point for downstream regression modeling on `arr_delay`.

---

## Dataset Overview

- **Source:** Kaggle — [Flight Data 2024](https://www.kaggle.com/datasets/hrishitpatil/flight-data-2024)
- **Size:** 100,000 rows sampled, 35 columns
- **Target Variable:** `arr_delay` — Arrival delay in minutes (positive = late, negative = early)
- **Goal:** Predict flight arrival delay in minutes based on scheduled timing, departure performance, and operational metrics.

---

## Data Cleaning Decisions

- Removed cancelled flights (`cancelled == 0`) — they have no arrival time
- Removed rows with missing `arr_delay` values
- Removed extreme outliers: delays below -100 or above +300 minutes (less than 0.3% of data)
- No duplicate rows were found

---

## Key Findings

- Flights that depart late almost always arrive late (`0.90` correlation between `dep_delay` and `arr_delay`)
- Delays compound throughout the day and peak around evening hours
- Florida airports (MIA, MCO) showed the highest average delays in the sample
- Distance and airtime had almost no relationship with delays
- Regional and premium carriers outperformed budget carriers on average

---

## Target Variable: `arr_delay`

`arr_delay` represents the difference in minutes between scheduled and actual arrival time.

- Positive values → flight arrived late
- Negative values → flight arrived early
- Zero → flight arrived exactly on time

Since `arr_delay` is continuous, predicting it is a regression task.

---

## Research Questions & Insights

### Q1: Which airlines have the highest average arrival delay?

Airlines differ significantly in scheduling discipline and operational efficiency. Comparing average arrival delay per carrier surfaces the most and least punctual operators.

[![Q1](https://huggingface.co/datasets/avihayamor/US_flight-delay-2024/resolve/main/q1_airlines.png)](/datasets/avihayamor/US_flight-delay-2024/blob/main/q1_airlines.png)

**Insight:** B6 (JetBlue) and NK (Spirit) post the highest average delays (~6–9 minutes late). YX (Republic Airways), 9E (Endeavor — Delta-owned regional), and DL (Delta) consistently arrive ahead of schedule. Budget and low-cost carriers run materially later than regional and full-service operators.

---

### Q2: Does the time of day affect arrival delays?

Late-day flights may inherit delay from earlier rotations — the "delay snowball" effect.

[![Q2](https://huggingface.co/datasets/avihayamor/US_flight-delay-2024/resolve/main/q2_time_of_day.png)](/datasets/avihayamor/US_flight-delay-2024/blob/main/q2_time_of_day.png)

**Insight:** Early morning flights (5–6am) arrive ahead of schedule, with low airport congestion. Average delay grows steadily through the day and peaks around 7pm, confirming the cascade hypothesis.

---

### Q3: Which departure airports have the highest average arrival delay?

[![Airport delay map](https://huggingface.co/datasets/avihayamor/US_flight-delay-2024/resolve/main/q3_map.png)](/datasets/avihayamor/US_flight-delay-2024/blob/main/q3_map.png)

**Insight:** Florida airports (MIA, MCO) are the most delay-prone, consistent with heavy tourist traffic and frequent summer thunderstorms. MSP, BOS, and ATL perform better on average.

[🗺️ Open the interactive map](https://huggingface.co/spaces/avihayamor/flight-delay-map-2024)

---

### Q4: How do numerical features correlate with arrival delay?

A correlation heatmap identifies the strongest numeric predictors of `arr_delay`.

[![Q4](https://huggingface.co/datasets/avihayamor/US_flight-delay-2024/resolve/main/q4_heatmap.png)](/datasets/avihayamor/US_flight-delay-2024/blob/main/q4_heatmap.png)

**Insight:** `dep_delay` correlates 0.90 with `arr_delay` — by far the strongest predictor. `carrier_delay` (0.61) and `late_aircraft_delay` (0.63) contribute meaningfully. Notably, `distance` and `air_time` show near-zero correlation with delays — flight length is not a meaningful driver of lateness.

---

## Limitations & Next Steps

**Limitations**
- The 100,000-row sample is drawn from a single year (2024) and may not generalize to years with different weather or operational patterns
- Cancelled flights were excluded, so the analysis describes delay among completed flights, not overall reliability
- `dep_delay` is highly predictive of `arr_delay` but is itself a downstream variable — useful for in-flight prediction, less so for pre-departure forecasting
- Carrier-level findings reflect 2024 only and should not be read as long-term performance rankings

**Next Steps**
- Build a regression model excluding `dep_delay` to isolate truly pre-departure predictors (origin airport, scheduled hour, carrier, day of week)
- Layer in weather data to test how much of the airport-level variation is structural vs. weather-driven
- Extend to multi-year data to separate stable carrier effects from year-specific operational shocks

---

📓 [View the full analysis notebook](https://colab.research.google.com/drive/10lKxNniqdxiz8zS1i4NnEQP8I9wqanWG#scrollTo=3cHhDnDYujc2)

## Author

**Avihay Amor** — [linkedin.com/in/avihay-amor](https://linkedin.com/in/avihay-amor)

## Files in this Dataset

- `flight_data_2024_sample.csv` — cleaned sample of 100,000 flights
- `q1_airlines.png` — bar chart: average delay by airline
- `q2_time_of_day.png` — bar chart: average delay by hour
- `q3_map.html` — interactive map: delay by departure airport
- `q4_heatmap.png` — correlation heatmap
