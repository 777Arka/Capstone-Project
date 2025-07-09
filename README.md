# Capstone-Project
📄 Capstone Project Report

Dynamic Pricing for Urban Parking Lots

Summer Analytics 2025 – Consulting & Analytics Club × Pathway


---

🔍 Introduction

Urban parking in metropolitan areas suffers from inefficiencies due to static pricing, leading to overcrowding or underutilization. This project aims to build a real-time, intelligent pricing engine for 14 urban parking spaces using live-streamed data. The goal is to set dynamic parking prices that optimize space utilization based on factors such as demand, congestion, and competition.


---

🗂 Dataset Overview

Time Coverage: 73 days

Time Interval: 18 timestamps/day (from 8:00 AM to 4:30 PM at 30-min intervals)

Features:

Latitude, Longitude – Geolocation

Capacity, Occupancy, QueueLen – Space usage

TrafficCond, IsSpecialDay – External conditions

VehicleType – Car, Bike, Truck

LastUpdatedTime – Timestamp of each observation




---

🧠 Models Implemented

🔹 Model 1: Baseline Linear Model

Pricing Formula:

P_{t+1} = P_t + \alpha \cdot \left( \frac{\text{Occupancy}}{\text{Capacity}} \right)

α (slope factor): Tuned empirically (e.g., α = 2)

Rationale: Captures basic occupancy-pressure relationship.

Clamping: Prices capped between $5 and $25 to avoid unreasonable values.



---

🔹 Model 2: Demand-Based Pricing Model

Demand Function:

\text{Demand} = \alpha \cdot \left( \frac{\text{Occupancy}}{\text{Capacity}} \right) + \beta \cdot \text{QueueLen} - \gamma \cdot \text{TrafficLevel} + \delta \cdot \text{IsSpecialDay} + \epsilon \cdot \text{VehicleTypeWeight}

P_t = \text{BasePrice} \cdot (1 + \lambda \cdot \text{NormalizedDemand})

α = 1.2 (Occupancy rate)

β = 0.5 (Queue length)

γ = 1.0 (Traffic)

δ = 2.0 (Special day)

ε = 0.7 (Vehicle type weight: Car = 1.0, Bike = 0.5, Truck = 1.5)

λ = 0.3 (price sensitivity)

Price Bound: 0.5× to 2× base price to ensure stability.



---

🔹 Model 3: Competitive Pricing Model (Optional Bonus)

Enhancements:

Considers competitor prices within a 1 km radius.

Adjusts own price based on competitor trends:

If full and competitors are cheaper → price slightly reduced.

If competitors are expensive → price increased to match market demand.


Geospatial Calculation: Used Haversine formula via geopy to compute distances.



---

📝 Assumptions

1. Vehicle Type Weighting: Heavier vehicles (trucks) are weighted more since they occupy more space and generate more demand.


2. Traffic Impact: Higher traffic reduces demand (discouraging travel).


3. Special Day Effect: Holidays/events increase demand due to public outings.


4. Demand Normalization: All demand scores were scaled between 0 and 1 to ensure pricing remained stable.


5. Pricing Clamps: Capped price changes to prevent irrational jumps that would confuse users.




---

🔄 Real-Time Simulation

Used Pathway to simulate real-time data ingestion:

Streaming time-ordered parking data

Hooked in Model 1 logic using UDFs

System outputs pricing in JSON format, ready for API deployment



---

📊 Visualizations

Used Bokeh for live visualizations:

Real-time price updates for a selected parking lot

Line graphs showing price evolution per time step

Visualization confirms price fluctuations match demand patterns



---

✅ Conclusion

This project successfully built three levels of pricing logic with increasing complexity:

From a simple rule-based model

To a multi-feature demand estimator

To a competitive pricing strategy


The pricing engine can be deployed using Pathway and visualized in Bokeh, making it suitable for real-time urban parking optimization.


---

📎 References

[https://pathway.com/developers/user-guide/introduction/first_realtime_app_with_pathway/
](https://pathway.com/developers/user-guide/deployment/from-jupyter-to-deploy/)

https://pathway.com/developers/user-guide/introduction/first_realtime_app_with_pathway/
