# 📞 Telecom CCaaS Data Analysis: A Maqsam Case Study

## 📌 Why I Built This Project
Honestly, this project started from pure curiosity. I wanted to understand how a CCaaS (Contact Center as a Service) business like Maqsam actually works behind the scenes. 

Telecom billing is unique. You have flat monthly subscriptions on one side, and minute-by-minute carrier routing costs on the other. I started wondering: how does a Data Scientist actually analyze data in this environment? How do they figure out if a customer is secretly losing the company money? How do they predict if a client is going to cancel their service?

Instead of just reading about it, I decided to build a simulation. I visited Maqsam's website, looked at their pricing model, generated my own realistic dataset, and set out to solve the exact kinds of daily problems a data team would face there. It was a great way for me to dive deep into the telecom domain and put my analytical skills to a real-world test.

## ⚙️ How I Modeled the Data (Business Assumptions)
To make this as close to reality as possible, I wrote a Python script (`generate_telecom_data.py`) to generate the data from scratch. I applied the following business rules based on my understanding of the CCaaS model:

* **The 2,000 Free Minutes Rule:** Every account gets 2,000 free inbound minutes per month. The financial analysis strictly accounts for this, only calculating costs and revenues after this threshold is passed.
* **The Rate Card:** I built a dual-cost system. Every call has a `rate_price` (what the customer pays us) and a `rate_cost` (what the telecom carrier charges us). I intentionally added some scenarios where the carrier cost is higher than the customer price to see if my analysis could catch the "negative margin" leak.
* **Seat Utilization:** A customer might pay a monthly fee for 50 seats, but a seat is only "active" if a distinct agent actually makes a call. 
* **Local Numbers (DIDs):** If a customer makes thousands of calls to Saudi Arabia but doesn't own a local Saudi number, they pay higher international rates and get lower answer rates.
* **SIP Error Codes:** 4xx codes mean normal user behavior (like the person didn't answer), but 5xx codes mean the carrier or the server actually failed.

## 🛠️ Tech Stack & Requirements
I used a modern data engineering stack to ensure the queries run fast and the files are lightweight.
* **Data Generation:** Python (Pandas, Faker)
* **Storage Engine:** Parquet (for columnar storage and fast reading)
* **Compute Engine:** DuckDB (for fast, in-memory SQL execution)
* **Analysis & Presentation:** Jupyter Notebook

### Installation
To run the code yourself, you will need to install these libraries:
```bash
pip install pandas duckdb pyarrow faker jupyter
```

## 📂 Project Structure
`generate_telecom_data.py`: The script that builds the fake customers and call logs based on the assumptions above.

`dim_customers.parquet & fact_calls.parquet `: The generated data files.

`profitability_analysis.ipynb` : The Jupyter Notebook containing the actual SQL queries and insights.

## 📊 The 4 Pillars of Analysis
Inside the notebook, I answered four main business questions:

**1. Finance (Margin Leaks):** I found which international routes are costing the company more money than they make.

**2. Customer Success (Churn Risk):** I calculated the real "seat utilization" to find customers who are paying for empty seats and are likely to cancel their subscription soon.

**3. Sales (Upsell Opportunities)**: I matched outbound call destinations with the phone numbers the customer owns to build a list of companies that need to buy a local DID number.

**4. Engineering (Platform Health):** I tracked the 5xx SIP error rates to make sure the carrier networks are stable and healthy.

## 🚀 How to Run It
Clone this repository to your machine.

**(Optional)** Run python generate_telecom_data.py if you want to generate a new set of random data.

Open profitability_analysis.ipynb and run the cells. DuckDB will query the Parquet files directly.
