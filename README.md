# sniper-insider-tracker
Sniper insider trading dashboard”
import streamlit as st
import pandas as pd
import numpy as np

# Sample data
DATA = [
    {
        "Date": "2025-07-17",
        "Ticker": "ABC",
        "Company": "Alpha Corp",
        "Insider": "John Doe",
        "Role": "CEO",
        "Buy Amount ($)": 150000,
        "Average Buy Price": 25.50,
        "Sniper Entry Price (~2% below)": 24.99,
        "Support Zone Noted?": "Yes",
        "Chart Link": "https://tradingview.com/ABC",
        "Status": "Watch"
    },
    {
        "Date": "2025-07-18",
        "Ticker": "XYZ",
        "Company": "Zeta Inc",
        "Insider": "Jane Smith",
        "Role": "CFO",
        "Buy Amount ($)": 200000,
        "Average Buy Price": 42.75,
        "Sniper Entry Price (~2% below)": 41.89,
        "Support Zone Noted?": "Yes",
        "Chart Link": "https://tradingview.com/XYZ",
        "Status": "Watch"
    },
]

df = pd.DataFrame(DATA)

# Add simulated current price and ROI
np.random.seed(42)
df["Current Price"] = df["Average Buy Price"] * (1 + np.random.uniform(-0.05, 0.05, size=len(df)))
df["ROI %"] = ((df["Current Price"] - df["Average Buy Price"]) / df["Average Buy Price"]) * 100

st.title("🔫 Sniper Insider Trading Dashboard")

st.markdown("""
Track insider buys, sniper entries, ROI, and manage your trades with ease.
""")

# Filter by Status and Role
status_filter = st.multiselect("Filter by Status", options=df["Status"].unique(), default=df["Status"].unique())
role_filter = st.multiselect("Filter by Role", options=df["Role"].unique(), default=df["Role"].unique())

filtered_df = df[(df["Status"].isin(status_filter)) & (df["Role"].isin(role_filter))]

# Display filtered table
st.dataframe(filtered_df.style.format({
    "Buy Amount ($)": "${:,.0f}",
    "Average Buy Price": "${:.2f}",
    "Sniper Entry Price (~2% below)": "${:.2f}",
    "Current Price": "${:.2f}",
    "ROI %": "{:.2f}%"
}))

st.markdown("---")

st.subheader("Summary")
st.write(f"Total Trades: {len(filtered_df)}")
st.write(f"Average ROI: {filtered_df['ROI %'].mean():.2f}%")