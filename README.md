# Morocco Electricity Consumption Analysis (Tetouan, 2017)

**AnalystLab Africa · Experience Lab Internship · Week 5 · Data Analytics track**
**Author:** Ada Eudoxie

An exploratory analysis of **52,416 electricity readings** taken every 10 minutes across three distribution zones in Tetouan, northern Morocco, from **1 January to 30 December 2017**. The project covers data inspection and cleaning, consumption patterns by hour, day and month, the relationship between demand and weather, and recommendations for energy planning, with charts in Matplotlib, Seaborn and Plotly.

> The dataset does not state a unit for consumption. I treated each reading as the average power demand during its 10-minute interval, not as energy used, so all comparisons use **average demand** rather than totals.

![Hourly demand profile](images/03%20-%20Hourly%20Profile.png)

## Headline results
| Measure | Zone 1 | Zone 2 | Zone 3 |
|---|---|---|---|
| Average demand per reading | **32,345** | **21,043** | **17,835** |
| Share of combined demand | 45% | 30% | 25% |
| Variability (coefficient of variation) | 22% | 25% | **37%** |
| Peak hour / lowest hour | 20:00 / 06:00 | 20:00 / 06:00 | 20:00 / 07:00 |
| Highest / lowest month | Aug / Nov | Aug / Apr | Jul / Dec |
| Sunday vs weekly average | -5% | -10% | +1% |
| Temperature correlation (daily Spearman) | **0.72** | 0.39 | 0.47 |

**Key findings:**
1. Zone 1 records the highest consumption and Zone 3 the lowest, but Zone 3 is the most variable.
2. All zones peak at 20:00. The evening peak is 89% above the early-morning low in Zones 1 and 2, and 123% above it in Zone 3.
3. Summer is the high-demand season. Zone 3's July average is about 2.5 times its December average.
4. Zone 2 has its own seasonal shape: lowest in April and rising again from October to December.
5. Weekdays are stable. Sunday is the lowest day in Zones 1 and 2, while Zone 3 barely changes across the week.
6. Temperature is the weather factor most associated with demand, and the link holds within the same hour of day. Humidity and wind speed show only weak associations, largely explained by time of day and season. These are associations, not proof of cause.

![Monthly demand profile](images/05%20-%20Monthly%20Profile.png)

## Recommendations
1. Plan peak capacity and reserves around **19:00 to 22:00 in June to August**, using each zone's own summer evening profile.
2. Introduce **demand-shifting measures** for the evening peak, such as time-of-use pricing and hot-day customer messages.
3. Schedule **maintenance in low-demand windows**: early mornings, Sundays for Zones 1 and 2, and September to December for Zone 3.
4. Build a **temperature-aware short-term forecast** for each zone, using the hourly and weekly profiles together with weather forecasts.
5. **Improve the data**: confirm the consumption unit and the meaning of the diffuse flow fields, check the wind sensor, and collect more years of data.

The full analysis is in the [notebook](notebooks/Morocco%20Electricity%20Analysis.ipynb) and the [report](reports/Morocco%20Electricity%20Consumption%20Report.pdf). The [interactive charts](reports/Interactive%20Charts.html) open in any browser once downloaded.

## Data preparation
- Converted `Datetime` to a date-time type and checked the intervals: every gap is exactly 10 minutes, with 144 readings on each of 364 days. 31 December 2017 is not in the data.
- Found no missing values, duplicate rows or duplicate timestamps. The cleaning code still handles them (de-duplication, re-indexing to a full 10-minute grid and interpolation of gaps up to one hour) in case the file is updated.
- Checked extreme values with the IQR rule. The 1,191 high Zone 3 readings all fall on July and August evenings and follow the daily curve, so I kept them as genuine summer demand.
- Flagged wind speed as unreliable because its readings cluster at two values (about 0.08 and about 4.9).
- Kept the two diffuse flow fields out of the conclusions because their meaning is not defined.
- Added Year, Month, Day, Hour, Day of week, Date and a weekend flag, plus hourly and daily averages.

## Repository structure
```
Morocco-Electricity-Consumption/
├── README.md
├── Requirements.txt
├── .gitignore
├── data/
│   └── powerconsumption.csv
├── notebooks/
│   └── Morocco Electricity Analysis.ipynb
├── images/
│   ├── 01 - Daily Trend.png
│   ├── 02 - Zone Average.png
│   ├── 03 - Hourly Profile.png
│   ├── 04 - Weekday Profile.png
│   ├── 05 - Monthly Profile.png
│   └── 06 - Environment Correlation.png
└── reports/
    ├── Morocco Electricity Consumption Report.pdf
    └── Interactive Charts.html
```

## How to reproduce
1. Clone the repository and install the requirements:
   ```bash
   git clone https://github.com/adaeudoxie/Morocco-Electricity-Consumption.git
   cd Morocco-Electricity-Consumption
   pip install -r Requirements.txt
   ```
2. Optionally, place the supplied `powerconsumption_datadictionary.csv` in `data/`. The notebook displays it when it is present.
3. Open `notebooks/Morocco Electricity Analysis.ipynb` in Jupyter and run all cells. The notebook cleans the data, builds the time features, runs the analysis and regenerates every chart in `images/` and `reports/Interactive Charts.html`.

The cleaning runs inside the notebook, so there is no separate cleaned file. Running it rebuilds the analysis-ready dataset from the raw CSV.

## Tools
Python · Jupyter Notebook · pandas · NumPy · Matplotlib · Seaborn · Plotly · SciPy

## Data source
Salam, A. and El Hibaoui, A. (2018). *Power Consumption of Tetouan City* [Dataset]. UCI Machine Learning Repository. CC BY 4.0.

#AnalystLabAfrica #DataAnalytics #Python
