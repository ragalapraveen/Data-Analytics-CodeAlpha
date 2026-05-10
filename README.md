## 📌 About the Project
This repository contains my completed tasks for the CodeAlpha Data Analytics Internship. The project covers Web Scraping, Exploratory Data Analysis (EDA), and Data Visualization using Python.

The goal is to extract raw data from a public website, clean and explore it, and present insights through clear visualizations.

## ✅ Tasks Completed

### *Task 1: Web Scraping*
- Scraped book titles and prices from [books.toscrape.com](https://books.toscrape.com) using Python + BeautifulSoup.
- Saved the extracted data as a structured CSV file books_dataset.csv.
- Implemented delays and headers to ensure ethical scraping.

### *Task 2: Exploratory Data Analysis (EDA)*
- Loaded and inspected the dataset to understand its structure and data types.
- Cleaned the price column by removing currency symbols and converting to numeric format.
- Identified missing values, duplicates, and statistical summaries like mean, min, and max price.
- Detected outliers in the price data using quantiles.

### *Task 3: Data Visualization*
- Created a dashboard with 3 key visualizations using Matplotlib & Seaborn:
 1. *Histogram* - Shows the overall distribution of book prices.
 2. *Bar Chart* - Displays the count of books in different price ranges.
 3. *Boxplot* - Highlights price outliers in the dataset.
- Designed visuals to tell a clear data story for decision-making.

## 📊 Key Insights
- Most books in the dataset are priced between £20-£40.
- Average book price is around £35.
- A small number of books are priced above £60, considered outliers.

## 🛠️ Tools & Libraries Used
- *Python 3.8+*
- *Requests* - For fetching webpage content
- *BeautifulSoup* - For parsing HTML and extracting data
- *Pandas* - For data cleaning and analysis  
- **
