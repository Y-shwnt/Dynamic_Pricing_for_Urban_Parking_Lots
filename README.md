# Dynamic_Pricing for Urban Parking Lots using Real-Time Streaming

This project implements **dynamic parking pricing models** using **real-time streaming data**, processed via [Pathway](https://pathway.com) and visualized with [Bokeh](https://bokeh.org). The objective is to optimize parking lot pricing dynamically based on demand signals such as occupancy volatility, queue length, traffic conditions, and more.

---

## Models Implemented

| Model | Name                     | Description                                                                                    |
| ----- | ------------------------ | ---------------------------------------------------------------------------------------------- |
| 1     | Linear Pricing           | Adjusts price based on daily fluctuations in occupancy rate.                                   |
| 2     | Demand-Based Pricing     | Prices based on weighted demand factors such as queue, traffic, special day, and vehicle type. |

---

## Tech Stack

| Category         | Tools/Libraries                           |
| ---------------- | ----------------------------------------- |
| Real-Time Engine | `pathway`                                 |
| Data Handling    | `pandas`, `numpy`                         |
| Visualization    | `bokeh`, `panel` |
| Environment      | Google Colab / Jupyter                    |
| Output Formats   | `CSV`, `JSONL`, `HTML`                    |

---

## Architecture Diagram

                     +---------------------+
                     |     dataset.csv     |
                     +---------------------+
                                ↓
                    +-------------------------+
                    | Preprocessing (Pandas)  |
                    +-------------------------+
                                ↓
             +--------------------------------------+
             | Stream via Pathway (CSV Replay)      |
             +--------------------------------------+
                  ↓                      ↓
        +----------------+       +--------------------+
        |  Model 1       |       |     Model 2        |
        |  Linear Price  |       | Demand-Based Price |
        +----------------+       +--------------------+
                  ↓                      ↓
            [ JSONL Output ]       [ JSONL Output ]
                  ↓                      ↓
        +------------------+    +----------------------+
        | Bokeh Dashboard  |    | Bokeh Dashboard      |
        +------------------+    +----------------------+

---

## Data Flow Summary

### Model 1: Linear Pricing

**Formula:**
```python
price = 10 + (max_occupancy_rate - min_occupancy_rate) / capacity
```

**Workflow:**
```
CSV ➝ Timestamp Merge ➝ OccupancyRate ➝ Pathway Tumbling Window ➝
Compute Max/Min ➝ Apply Price Formula ➝ JSONL Output ➝ Bokeh/Panel
```

**Model 1 Visuals:**

![Model 1 Trend](outputs/Model1.png)
![Model 1 Combined](outputs/Model1_Combined.png)

---

### Model 2: Demand-Based Pricing

**Demand Formula:**
```python
demand = 1*occ + 0.5*queue - 0.2*traffic + 2*special + 0.8*vehicle
```

**Price Formula:**
```python
price = base * (1 + lambda * min(demand / max_demand, 1))
```

**Workflow:**
```
CSV ➝ Timestamp Merge ➝ Feature Encoding ➝ Pathway Tumbling Window ➝
Aggregate Demand Components ➝ Compute Demand ➝ Normalize ➝
Compute Price ➝ JSONL Output ➝ Bokeh/Panel
```

**Model 2 Visual:**

![Model 2 Trend](outputs/Model2.png)
![Model 2 Combined](outputs/Model2_Combined.png)

---

## Interactive HTML Outputs

| File                                  | Description                          |
| ------------------------------------- | ------------------------------------ |
| `outputs/model1_price_trends.html`    | Lot-wise trends from Model 1         |
| `outputs/model2_price_trends.html`    | Lot-wise trends from Model 2         |
| `outputs/daily_pricing_comparison.html` | Combined trends across lots (Model 2) |

---

## Notebooks Included

- `notebooks/Baseline_Linear_Model.ipynb`
- `notebooks/Demand_Based_Pricing_Model.ipynb`

---
## CSV Pricing Outputs

| File                  | Description                      |
|-----------------------|----------------------------------|
| `outputs/pricing_output_1.csv`  | Final price output from Model 1 (Linear-Based)   |
| `outputs/pricing_output_2.csv`  | Final price output from Model 2 (Demand-Based)   |

---

## Project Status

- [x] Real-time stream simulation with Pathway
- [x] Model 1: Volatility-based pricing
- [x] Model 2: Demand-based pricing
- [x] Bokeh-based dashboard visualization
- [x] HTML/JSONL output export

---

## Future Improvements

- **API Integration** – Stream real-time data via WebSocket or REST.
- **Smarter Geospatial Logic** – Use GPS data and road network intelligence for improved accuracy.
- **Advanced Pricing Models** – Integrate machine learning or reinforcement learning for dynamic pricing.
- **Better Visualizations** – Add interactive dashboards with filters, heatmaps, and drill-down analytics.
- **Historical Logging** – Maintain historical pricing data for long-term analysis and insights.

---
