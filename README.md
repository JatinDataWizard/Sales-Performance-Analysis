## Data Cleaning Script (data_cleaning.py)
-This script will load the data, clean it, and save the cleaned data to a new CSV file.
import pandas as pd

def clean_data(file_path):
    # Load the data
    df = pd.read_csv(file_path)

    # Display initial data info
    print("Initial Data Info:")
    print(df.info())

    # Drop duplicates
    df.drop_duplicates(inplace=True)

    # Convert 'Date' column to datetime
    df['Date'] = pd.to_datetime(df['Date'])

    # Fill missing values (if any)
    df.fillna({'Quantity': 0, 'Price': 0}, inplace=True)

    # Save the cleaned data
    cleaned_file_path = 'data/cleaned_sales_data.csv'
    df.to_csv(cleaned_file_path, index=False)
    print(f"Cleaned data saved to {cleaned_file_path}")

if __name__ == "__main__":
    clean_data('data/sales_data.csv')

##Data Visualization Script (data_visualization.py)
-This script will create visualizations based on the cleaned data.
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

def visualize_data(file_path):
    # Load the cleaned data
    df = pd.read_csv(file_path)

    # Set the style of seaborn
    sns.set(style="whitegrid")

    # Sales over time
    df['Total_Sales'] = df['Quantity'] * df['Price']
    sales_over_time = df.groupby('Date')['Total_Sales'].sum().reset_index()

    plt.figure(figsize=(12, 6))
    sns.lineplot(data=sales_over_time, x='Date', y='Total_Sales')
    plt.title('Total Sales Over Time')
    plt.xlabel('Date')
    plt.ylabel('Total Sales')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.savefig('sales_over_time.png')
    plt.show()

    # Top products
    top_products = df.groupby('Product')['Total_Sales'].sum().nlargest(10).reset_index()

    plt.figure(figsize=(12, 6))
    sns.barplot(data=top_products, x='Total_Sales', y='Product', palette='viridis')
    plt.title('Top 10 Products by Total Sales')
    plt.xlabel('Total Sales')
    plt.ylabel('Product')
    plt.tight_layout()
    plt.savefig('top_products.png')
    plt.show()

if __name__ == "__main__":
    visualize_data('data/cleaned_sales_data.csv')
    
## Jupyter Notebook (sales_analysis.ipynb)
-You can create a Jupyter Notebook to run the analysis interactively. Below is a sample structure of what the notebook might contain:
# Sales Performance Analysis

## 1. Import Libraries
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

## Load Cleaned Data
df = pd.read_csv('data/cleaned_sales_data.csv')

## Data Overview
 df.head()
 df.describe()
## Visualize Total Sales Over Time
 df['Total_Sales'] = df['Quantity'] * df['Price']
 sales_over_time = df.groupby('Date')['Total_Sales'].sum().reset_index()

 plt.figure(figsize=(12, 6))
 sns.lineplot(data=sales_over_time, x='Date', y='Total_Sales')
 plt.title('Total Sales Over Time')
 plt.xlabel('Date')
 plt.ylabel('Total Sales')
 plt.xticks(rotation=45)
 plt.show()
## Visualize Top Products
 top_products = df.groupby('Product')['Total_Sales'].sum().nlargest(10).reset_index()

 plt.figure(figsize=(12, 6))
 sns.barplot(data=top_products, x='Total_Sales', y='Product', palette='viridis')
 plt.title('Top 10 Products by Total Sales')
 plt.xlabel('Total Sales')
 plt.ylabel('Product')
 plt.show()


  


    
