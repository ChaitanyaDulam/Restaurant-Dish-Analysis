# Restaurant Dish Analysis
This project was completed as part of the final assignment for the **BUAN 6333 (Business Analytics with R and Python)** course at the University of Texas at Dallas.

## Project Overview
The analysis focuses on a restaurant's menu data that includes information about dishes, cuisines, prices, costs, and customer ratings. The primary objective is to uncover insights through data exploration and visualization using Python.

### Scenario
Analyze data related to a restaurant’s dishes from their online menu, focusing on trends in cuisine, pricing, customer ratings, and cooking costs.

## Dataset Description
The dataset contains the following attributes:

| Column         | Description                                           |
|----------------|-------------------------------------------------------|
| `date`         | Date on which a dish became available                 |
| `dish`         | Name of the dish                                      |
| `cuisine`      | Type of cuisine (e.g., Italian, Indian, etc.)         |
| `type`         | Dish type (e.g., appetizer, entree, dessert)          |
| `rating`       | Customer rating for the dish (scale of 1–5)           |
| `price`        | Selling price of the dish (USD)                       |
| `cooking_cost` | Cost incurred to prepare the dish (USD)               |

## Technologies Used
- Python 3.11+
- Jupyter Notebook
- Pandas
- Matplotlib / Seaborn
- Numpy

## Key Insights & Analysis
- Cuisine popularity based on customer ratings
- Price vs. cooking cost profitability analysis
- Distribution of dish types and seasonal availability
- Rating trends by dish type and cuisine
## CODE
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import os

# Data Cleaning
# Missing value count before cleaning
print(" Missing values before cleaning:")
print(df.isnull().sum())

# Convert date column to datetime
df['date'] = pd.to_datetime(df['date'], errors='coerce')

# Check missing values after coercion
print("\n Missing values after date conversion:")
print(df.isnull().sum())

# Drop rows with missing date
df = df.dropna(subset=['date'])
print(f"\n Rows remaining after dropping missing dates: {len(df)}")

# Fill missing values for other columns
df['rating'].fillna(df['rating'].mode()[0], inplace=True)
df['price'].fillna(df['price'].mean(), inplace=True)
df['cooking_cost'].fillna(df['cooking_cost'].mean(), inplace=True)

# Confirm no missing values remain
print("\n Missing values after cleaning:")
print(df.isnull().sum())

# Check how many completely duplicate rows exist
duplicate_count = df.duplicated().sum()
print(f" Number of duplicate rows: {duplicate_count}")

# Extract month in YYYY-MM format
df['month'] = df['date'].dt.to_period('M').astype(str)

# Count number of orders per month
monthly_orders = df['month'].value_counts().sort_index()

# Count number of orders per day
daily_order_counts = df['date'].value_counts().sort_index()

categorical_cols = df.select_dtypes(include=['object']).columns

for col in categorical_cols:
    print(f"\n Value counts for '{col}':")
    print(df[col].value_counts(dropna=False))

# Bar Chart Dish Frequency:
    # Get value counts
dish_counts = df['dish'].value_counts()

# Plot bar chart
plt.figure(figsize=(10, 8))
bars = plt.bar(dish_counts.index, dish_counts.values, color='orange')

# Add count labels on top of each bar
for bar in bars:
    height = bar.get_height()
    plt.text(
        bar.get_x() + bar.get_width() / 2,  # X position (centered)
        height + 10,                        # Y position (just above the bar)
        f'{int(height)}',                   # Label text
        ha='center', va='bottom', fontsize=12
    )

plt.title('Dish Frequency', fontsize=16)
plt.xlabel("Dish", fontsize=14)
plt.ylabel("Count", fontsize=14)
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()

# Box plot - Price by dish:
plt.figure(figsize=(12, 8))  # Increase the size of the plot
sns.boxplot(x='dish', y='price', data=df, palette='Set3')

plt.title('Boxplot of Price by Dish', fontsize=16)
plt.xlabel('Dish', fontsize=14)
plt.ylabel('Price', fontsize=14)
plt.xticks(rotation=45, ha='right', fontsize=12)  # Rotate labels for better readability
plt.tight_layout()
plt.show()

# Line plot price overtime
plt.figure(figsize=(12, 8))
df_sorted = df.sort_values(by='date')
sns.lineplot(x='date', y='price', data=df_sorted, color='orange')
plt.title('Line Plot of Price Over Time')
plt.tight_layout()
plt.show()

# Scatter plot Profit vs Price:
# Calculate profit
df['profit'] = df['price'] - df['cooking_cost']

# Create the scatter plot
plt.figure(figsize=(12, 8))
sns.scatterplot(x='cooking_cost', y='profit', hue='dish', data=df, palette='Set2')
plt.title('Scatter Plot of Cooking Cost vs Profit')
plt.xlabel('Cooking Cost')
plt.ylabel('Profit')
plt.tight_layout()
plt.show()

# Heat Map of Dish vs Rating:
# Create a cross-tab (like a pivot table) of dish vs cuisine
heatmap_data = pd.crosstab(df['dish'], df['rating'])

plt.figure(figsize=(10, 6))
sns.heatmap(heatmap_data, annot=True, fmt='d', cmap='YlGnBu')

plt.title('Heatmap of Dish and Rating', fontsize=16)
plt.xlabel('Rating', fontsize=14)
plt.ylabel('Dish', fontsize=14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.tight_layout()
plt.show()

