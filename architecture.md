⚙️ Detailed Workflow
1. Data Ingestion
Source: A static CSV dataset containing:

Parking lot ID, latitude/longitude

Timestamps sampled every 30 minutes

Occupancy, capacity, queue length

Traffic congestion, vehicle type, special day/event

Tool: Pathway is used to simulate a real-time streaming feed.

2. Feature Engineering
At each time step, features are processed:

Occupancy Rate = Occupancy / Capacity

Queue Length, Traffic Congestion, Special Day Indicator

Vehicle type is encoded (e.g., car = 1.0, bike = 0.5, truck = 1.5)

3. Pricing Models
Model 1: Baseline Linear Pricing
A reference model where price increases proportionally to occupancy:

python
Copy
Edit
Price_t+1 = Price_t + α * (Occupancy / Capacity)
Model 2: Demand-Based Pricing
A smarter approach using multiple weighted features to estimate demand:

python
Copy
Edit
Demand = α * (Occupancy / Capacity)
       + β * Queue Length
       - γ * Traffic Congestion
       + δ * Special Day
       + ε * Vehicle Type Weight
Then:

python
Copy
Edit
Price_t = Base_Price * (1 + λ * Normalized_Demand)
Price is clamped between 0.5× and 2× base price

Helps prevent erratic changes

Model 3: Competitive Pricing (Optional)
Incorporates pricing intelligence from neighboring lots:

Compute geographic proximity (via lat-long haversine distance)

Compare price with nearby lots

If lot is full and others are cheaper → suggest rerouting or decrease price

If others are expensive → price can increase slightly

#Visualization Layer
Tool: Bokeh

Functionality:

Live updating line charts per parking lot

Compare baseline vs. demand-based pricing

Show competitor prices

dynamic-parking-pricing/
├── data/                  → dataset.csv
├── project_code.ipynb     → Jupyter notebooks for all models
├── architecture.png       → architecture_diagram.png
├── visualizations/        → Optional saved plots
├── architecture.md        → This file
├── requirements.txt       → Python dependencies
└── README.md              → Project summary
