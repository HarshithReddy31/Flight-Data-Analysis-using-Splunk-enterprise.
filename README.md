# ✈️ Flight Operations and Delay Analysis with Splunk Enterprise

---

## 👨‍💻 Author

* **Sai Harshith Reddy Gaddamidhi** - *Project Lead & Splunk Analyst*
  

---

## Project Overview

This project showcases powerful **flight data analytics and visualization** using **Splunk Enterprise**. The goal was to transform raw operational data into actionable insights, focusing on flight delays, cancellations, route efficiency, and overall airline performance.

The analysis is presented through two key dashboards: **Delay Trends and Analysis** and **Navigating the Skies**, providing both micro-level delay causation and macro-level operational statistics.

### Key Objectives Achieved:

* **Determine Root Causes of Delay:** Quantify the total impact of different delay types (e.g., Late Aircraft, Carrier, NAS).
* **Evaluate Airline Performance:** Compare airlines based on on-time arrival rates and cancellation percentages.
* **Analyze Route Efficiency:** Identify the busiest routes and calculate average flight duration for specific origins.
* **Operational Monitoring:** Track key performance indicators (KPIs) like Total Flights, Cancellations, and Diverted Flights.

---

## 🛠️ Technology Stack

| Component | Role |
| :--- | :--- |
| **Splunk Enterprise** | Data ingestion, indexing, Search Processing Language (SPL) execution, and custom dashboard development. |
| **SPL (Search Processing Language)** | The primary language used for data manipulation, aggregation, and analysis. |
| **Data Source** | Anonymized historical flight operations data (similar to US DOT/BTS data). |

---

## 💡 Key Insights & Findings

The project revealed critical insights into flight operations:

### 1. Delay Trends & Causes (Analysis Dashboard)

| Delay Cause | Total Delay Units | Key Finding |
| :--- | :--- | :--- |
| **Late Aircraft** | $\mathbf{4,762,266}$ | The single largest contributor to total delay time. Mitigating turn-around time is critical. |
| **Carrier Delay** | $\mathbf{4,619,689}$ | Closely following Late Aircraft, pointing to issues with airline-specific operations and logistics. |
| **NAS Delay** | $\mathbf{2,456,052}$ | Significant delays caused by air traffic control, weather, or security, often external to the airline. |

* **Distance vs. Air Time:** The scatter plot shows a strong linear correlation between flight distance and air time, as expected, but also highlights outliers that may represent flights affected by unusual wind patterns or ground delays.
* **Longest Route from Boston (MA):** The route from **Boston, MA** to **Honolulu, HI** has the longest average flight duration ($\mathbf{638.52 \text{ minutes}}$).

### 2. Operational & Performance Metrics (Navigation Dashboard)

* **Total Operations:** The dataset spans over $\mathbf{1 \text{ Million}}$ flights ($\mathbf{1,048,575}$), with an average flight duration of $\mathbf{112.38 \text{ minutes}}$.
* **High-Volume Issues:** A total of $\mathbf{27,588}$ flights were cancelled and $\mathbf{2,489}$ were diverted.

#### Airline Performance Snapshot:

| Metric | Best Performer | Worst Performer (by volume/rate) |
| :--- | :--- | :--- |
| **On-Time Arrival Rate** | **Endeavor Air Inc.** (Score: 75) | *(Varies)* |
| **Cancellation Rate** | Many airlines at $\mathbf{0\%}$ to $\mathbf{1\%}$ | **Expressjet Airlines** ($\mathbf{5.5\%}$ cancellation rate) |

* **Busiest Routes:** The top five busiest routes are clearly mapped and visualized, led by high-traffic corridors like **New York, NY $\leftrightarrow$ Chicago, IL** and **Boston, MA $\leftrightarrow$ Atlanta, GA**.

---

## ⚙️ Example SPL (Search Processing Language)

The following simplified SPL examples illustrate the power of Splunk's query language used in this project:

```splunk
# Query to calculate total delay by cause (for the bar chart)
index=flights 
| stats sum(LateAircraftDelay) as Late_Delay, sum(CarrierDelay) as Carrier_Delay, sum(NASDelay) as NAS_Delay, ... 
| transpose header_field="Delay_Cause" 
| rename "row 1" as Total_Delay 

# Query to find the Busiest Routes (for the pie chart)
index=flights 
| stats count by ORIGIN_CITY, DEST_CITY 
| eval Route=ORIGIN_CITY . " -> " . DEST_CITY 
| sort -count 
| head 15
