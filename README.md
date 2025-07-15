# Dynamic Pricing for Urban Parking Lots

### Overview

Urban parking spaces are limited and highly demanded. Static pricing often leads to overcrowding or underutilization. This project presents a real-time, intelligent pricing engine for 14 urban parking lots using simulated live data. Prices adapt dynamically based on occupancy, traffic, queue lengths, event days, vehicle types, and even competition from nearby lots.

My models are implemented using only NumPy, Pandas, and Pathway, simulating a fully streaming data environment with real-time visualization using Bokeh.

### 🛠️ Tech Stack
| Tool                  | Purpose                               |
|-----------------------|---------------------------------------|
| Python (NumPy, Pandas)| Data manipulation, modeling           |
| Pathway               | Real-time data ingestion and streaming|
| Bokeh                 | Real-time interactive visualizations  |
| Google Colab          | Code development and execution platform|

### Architecture Diagram

```mermaid
graph TD
    A[dataset.csv: 73 days x 18 time slots] -->|Streaming input| B(Pathway Data Ingestor)
    B --> C(Feature Extractor)
    C --> D1[Model 1: Linear Occupancy-based Pricing]
    C --> D2[Model 2: Demand Function-based Pricing]
    C --> D3[Model 3: Competitive Pricing (Optional)]
    D1 --> E[Price Prediction Stream]
    D2 --> E
    D3 --> E
    E --> F[Real-Time Pricing Visualization (Bokeh)]
    E --> G[Suggested Rerouting Logic]
```

### Project Architecture & Workflow

** Dataset**

- 14 parking lots, 73 days, 18 half-hour time points (8:00 AM – 4:30 PM daily)
- **Features:**
    - **Lot info:** lat-long, capacity, occupancy, queue
    - **Vehicle:** car/bike/truck
    - **Environment:** congestion level, special day

** Model Pipeline**

- **Model 1 (Baseline):** Price increases linearly with occupancy.
  
  `Price_t+1 = Price_t + α * (Occupancy / Capacity)`

- **Model 2 (Demand-Based):**
  
  `Demand = α*(Occupancy/Capacity) + β*Queue - γ*Traffic + δ*SpecialDay + ε*VehicleWeight`
  
  `Price_t = BasePrice * (1 + λ * NormalizedDemand)`

- **Model 3 (Competitive Pricing) (Optional):**
    - Considers competitor prices using spatial proximity.
    - Enables rerouting suggestions if nearby lots are cheaper/available.

** Real-Time Simulation**

- Implemented via Pathway, simulating delayed streaming data.
- Emulates real-world ingestion and prediction, step-by-step.

** Visualization**

- Interactive Bokeh plots for:
    - Individual lot pricing vs. time
    - Competitor comparison
    - Demand fluctuations

** Rerouting Logic (Optional)**

- Suggests redirecting vehicles if the current lot is full and nearby alternatives are cheaper.

###  File Structure

```
.
├── Car Parking.ipynb        # Main notebook with models & visualization
├── dataset.csv              # Input dataset (14 parking lots, 73 days, 18 time points)
├── problem statement.pdf    # Original project brief and requirements
└── README.md                # This file
```

### Additional Documentation

- All models are built from scratch without using any ML libraries.
- **Assumptions made:**
    - Base price is $10
    - Demand normalization bounds prices between $5 and $20
    - Vehicle types are assigned weights: car=1.0, bike=0.7, truck=1.5
    - Congestion and special day indicators scaled appropriately.

### Report (Optional)

You can include a PDF report with model derivations and graphs.

### Working Code

The notebook:
- Is fully implemented and functional.
- Includes all 3 models with flexibility to switch between them.
- Shows real-time pricing with simulated data.
- Handles edge cases like full lots, extreme congestion, and special events.

**To run:**
```bash
# In Google Colab:
!pip install pathway bokeh
# Then run all cells in Car Parking.ipynb
```
