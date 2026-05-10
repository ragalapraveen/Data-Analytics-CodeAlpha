Task 1 : Web Scraping
!pip install requests beautifulsoup4 pandas
import requests
import time # Import the time module

url = "https://books.toscrape.com/catalogue/page-1.html"
response = requests.get(url)

print(response.status_code)  # 200 means success
from bs4 import BeautifulSoup

soup = BeautifulSoup(response.text, 'html.parser')
books = soup.find_all('article', class_='product_pod')  # each book is inside this tag

titles = []
prices = []

for book in books:
    title = book.h3.a['title']  # get title from 'title' attribute
    price = book.find('p', class_='price_color').text  # get price text

    titles.append(title)
    prices.append(price)
    import pandas as pd

data = {'Book Title': titles, 'Price': prices}
df = pd.DataFrame(data)

df.to_csv('books_dataset.csv', index=False)

print("Done! File saved as books_dataset.csv")
print(df.head())  # shows first 5 rows
all_titles = []
all_prices = []

for page in range(1, 3):  # scrape page 1 and 2 only
    url = f"https://books.toscrape.com/catalogue/page-{page}.html"
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    books = soup.find_all('article', class_='product_pod')

    for book in books:
        all_titles.append(book.h3.a['title'])
        all_prices.append(book.find('p', class_='price_color').text)

    time.sleep(2)  # wait 2 seconds so we don’t overload the site

df = pd.DataFrame({'Book Title': all_titles, 'Price': all_prices})
df.to_csv('books_dataset_full.csv', index=False)  

Task 2 : Exploratory Data Analysis (EDA)
import pandas as pd
import matplotlib.pyplot as plt

# Load the CSV file
df = pd.read_csv('books_dataset.csv')

# First 5 rows — what does the data look like?
print(df.head())

# How many rows and columns?
print("Shape:", df.shape)
# Show column names and data types
print(df.info())

# Show basic statistics for number columns
print(df.describe())
df['Price'] = df['Price'].str.replace('Â£','').astype(float)
# Most expensive and cheapest book
print("Most expensive:", df['Price'].max())
print("Cheapest:", df['Price'].min())

# Average price
print("Average price:", df['Price'].mean())

# Check for missing values
print("Missing values:\n", df.isnull().sum())
# Histogram of prices
plt.hist(df['Price'], bins=20, color='skyblue', edgecolor='black')
plt.title('Distribution of Book Prices')
plt.xlabel('Price (£)')
plt.ylabel('Number of Books')
plt.show()
cheap_books = df[df['Price'] < 30]
print(f"Books under £30: {len(cheap_books)} out of {len(df)}")
# Check for duplicate book titles
duplicates = df[df.duplicated(subset=['Book Title'])]
print("Duplicate books:", len(duplicates))

# Check for outliers - books much more expensive than others
outliers = df[df['Price'] > df['Price'].quantile(0.95)]
print("Possible outliers:\n", outliers[['Book Title', 'Price']].head())

Task 3 : Data Visuvalization 
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load and clean
df = pd.read_csv('books_dataset.csv')
df['Price'] = df['Price'].str.replace('Â£','').astype(float)
df['Price Range'] = pd.cut(df['Price'], bins=[0, 20, 40, 60, 100],
                           labels=['Cheap 0-20', 'Medium 20-40', 'Expensive 40-60', 'Very Expensive 60+'])

# Dashboard with 3 charts
fig, axes = plt.subplots(1, 3, figsize=(15,4))

sns.histplot(df['Price'], bins=20, ax=axes[0], color='skyblue', kde=True)
axes[0].set_title('Price Distribution')

sns.countplot(data=df, x='Price Range', ax=axes[1], palette='pastel')
axes[1].set_title('Books by Price Range')
axes[1].tick_params(axis='x', rotation=20)

sns.boxplot(y=df['Price'], ax=axes[2], color='lightgreen')
axes[2].set_title('Price Outliers')

plt.tight_layout()
plt.savefig('books_dashboard.png') # saves image for your portfolio
plt.show()

print("Dashboard saved as books_dashboard.png")

