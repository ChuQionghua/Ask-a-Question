# Ask-a-Question
# Data Preprocessing

# Importing the libraries
import numpy as np
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt
import pandas as pd
import scipy.stats as stats

# Importing the dataset
df = pd.read_csv('USDJPY.csv', dayfirst = True, parse_dates = True,  index_col = 0) # Must check the date format
df = df.sort_index()

# Creating new column in pandas
df['Price Change'] = df['Last Price'].pct_change()*100
df = df.dropna()

# Histogram
pc = df['Price Change'].dropna()
n, bins, patches = plt.hist(pc, bins = np.arange(round(min(df['Price Change']), 1) - 0.1, 
                                                round(max(df['Price Change']), 1) + 0.1, 
                                                0.1),
                            normed = 1, facecolor='green', alpha=0.75)

# add a 'best fit' line
y = mlab.normpdf(bins, np.mean(pc), np.std(pc))
l = plt.plot(bins, y, 'r--', linewidth=1)

plt.xlabel('Change')
plt.ylabel('Count')
plt.title('Daily Price Percent Change')
plt.grid(True)

plt.show()

print(df['Price Change'].describe())


def price_change(a, b):
    if a > b:
        upper = a; lower = b
    elif b > a:
        upper = b
        lower = a
    elif a == b: print("Please give a range.")
    else:
        print("Something wrong happened")
    
    # Importing the dataset
    df = pd.read_csv('USDJPY.csv', dayfirst = True, parse_dates = True,  index_col = 0)
    df = df.sort_index()
    
    # Creating new column in pandas
    df['Price Change'] = df['Last Price'].pct_change()*100
    df = df.dropna()
    
    df['P_D'] = df['Price Change'].shift(-1)
    df['P_W'] = df['Last Price'].pct_change(periods = 5)*100
    df['P_M'] = df['Last Price'].pct_change(periods = 22)*100
    df = df[(df['Price Change'] >= lower) & (df['Price Change'] <= upper)]
    
    print(
    "\nWhen the price changed between", lower,"% and", upper,"% within a single day,")
    print(
    "\nPrice changes:",
    "\nAfter 1 Day --- Average =", round(df['P_D'].mean(), 2), "%, SD =",round(df['P_D'].std(), 2), "%",
    "\nAfter 1 Week -- Average =", round(df['P_W'].mean(), 2), "%, SD =",round(df['P_W'].std(), 2), "%",
    "\nAfter 1 Month - Average =", round(df['P_M'].mean(), 2), "%, SD =",round(df['P_M'].std(), 2), "%"
    )
    
    print("\nThis happened", df['Price Change'].count(), "times in the past 30 years")
    
    print("\nThe last time this happened was on", df.index[-1])

price_change(1.5, 1.6)
