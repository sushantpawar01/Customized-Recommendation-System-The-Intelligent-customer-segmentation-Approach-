
# Retail Customers Segmentation & Recommendation System

![Python](https://img.shields.io/badge/Python-3.10-blue.svg)
![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange.svg)
![Status](https://img.shields.io/badge/Status-Completed-green.svg)

## 📌 Project Overview
In the competitive online retail sector, personalized marketing is key to maximizing sales and customer retention. This project analyzes a transactional dataset from a UK-based retailer (2010-2011) to perform **Customer Segmentation** and build a **Product Recommendation System**.

By transforming transactional data into a customer-centric dataset, we utilized the **K-Means clustering** algorithm to identify distinct customer profiles. Based on these segments, a recommendation engine was developed to suggest top-selling products to customers who haven't purchased them yet.

## 📂 Dataset
The dataset represents transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based and registered non-store online retail. 
* **Source:** [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/352/online+retail)
* **Size:** 541,909 entries

| Variables | Description |
| :--- | :--- |
| **InvoiceNo** | Unique transaction code (starts with 'c' if cancelled). |
| **StockCode** | Unique product code. |
| **Description** | Product name. |
| **Quantity** | Number of products per transaction. |
| **InvoiceDate** | Date and time of transaction. |
| **UnitPrice** | Product price per unit (Sterling). |
| **CustomerID** | Unique customer identifier. |
| **Country** | Customer's country of residence. |

## 🛠️ Project Workflow

### 1. Data Cleaning & Transformation
* **Missing Values:** Removed rows with missing `CustomerID` (essential for segmentation) and `Description`.
* **Duplicates:** Removed duplicate transaction entries.
* **Anomalies:** Filtered out stock codes indicating non-product transactions (e.g., 'POST', 'BANK CHARGES') and zero unit prices.
* **Cancellations:** Retained but flagged cancelled transactions to analyze return behaviors.

### 2. Feature Engineering
We transformed the transactional data into a customer-centric dataset with the following engineered features:
* **RFM Metrics:** Recency (Days since last purchase), Frequency (Total transactions), Monetary (Total spend).
* **Behavioral:** Average days between purchases, favorite shopping day, favorite shopping hour.
* **Product Diversity:** Number of unique products purchased.
* **Geographic:** Binary flag for UK vs. Non-UK customers.
* **Cancellations:** Cancellation frequency and rate.
* **Seasonality:** Monthly spending mean and standard deviation, spending trends.

### 3. Outlier Detection
* Applied **Isolation Forest** to identify and remove anomalies in the multi-dimensional feature space (~5% of data removed).

### 4. Preprocessing & Dimensionality Reduction
* **Correlation Analysis:** Identified multicollinearity among features.
* **Scaling:** Applied `StandardScaler` to normalize features.
* **PCA (Principal Component Analysis):** Reduced dimensions to 6 components, retaining **81%** of the variance.

### 5. K-Means Clustering
* **Optimal K:** Determined using the **Elbow Method** and **Silhouette Analysis**.
* **Result:** The optimal number of clusters was determined to be **3**.

## 📊 Customers Segments (Results)
Using Radar Charts and Histograms, we profiled the 3 distinct clusters:

| Cluster | Profile Name | Characteristics |
| :--- | :--- | :--- |
| **0** | **Sporadic Shoppers** | • Low spend & transaction count.<br>• Preference for weekend shopping.<br>• Low cancellation rate.<br>• Stable but low spending trend. |
| **1** | **Infrequent Big Spenders** | • High average transaction value but infrequent purchases.<br>• Increasing spending trend over time.<br>• Shop late in the day.<br>• Moderate cancellation rate. |
| **2** | **Frequent High-Spenders** | • High total spend & unique products purchased.<br>• Very frequent transactions (low days between purchases).<br>• High cancellation frequency.<br>• Shop early in the day. |

## 💡 Recommendation System
A targeted recommendation system was built for the 95% of non-outlier customers:
1.  **Logic:** Identify top-selling products within each specific cluster.
2.  **Filtering:** Exclude products the specific customer has already purchased.
3.  **Output:** Suggest the **top 3 items** popular in their cluster to encourage cross-selling.
