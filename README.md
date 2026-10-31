# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load the BMW car sales dataset
data = pd.read_csv('/content/BMW_Car_Sales_Classification.csv')

print("Original Data:")
print(data.head())

# Aggregate sales volume by year
resampled_data = (
    data.groupby('Year')['Sales_Volume']
    .sum()
    .reset_index()
)

print("\nYearly Sales Volume:")
print(resampled_data.head())

# Extract years and sales volume
years = resampled_data['Year'].tolist()
sales = resampled_data['Sales_Volume'].tolist()

# Create centered X values
X = [year - years[len(years) // 2] for year in years]

# Linear Trend Estimation
x2 = [x**2 for x in X]
xy = [x * y for x, y in zip(X, sales)]

n = len(years)

if n < 3:
    raise ValueError("At least 3 distinct years are needed for a degree-2 polynomial trend.")

denominator = n * sum(x2) - sum(X)**2

if denominator == 0:
    raise ValueError("Cannot calculate the linear trend with these year values.")

b = (n * sum(xy) - sum(sales) * sum(X)) / denominator
a = (sum(sales) - b * sum(X)) / n

linear_trend = [a + b * x for x in X]

# Polynomial Trend Estimation (Degree 2)
x3 = [x**3 for x in X]
x4 = [x**4 for x in X]
x2y = [x2[i] * sales[i] for i in range(n)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(sales), sum(xy), sum(x2y)]

solution = np.linalg.solve(np.array(coeff, dtype=float),
                           np.array(Y, dtype=float))

a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * x + c_poly * x**2
    for x in X
]

# Display trend equations
print(f"\nLinear Trend: y = {a:.2f} + {b:.2f}x")
print(
    f"Polynomial Trend: y = {a_poly:.2f} "
    f"+ {b_poly:.2f}x + {c_poly:.2f}x²"
)

# Add trends to the dataframe
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

# Visualize the results
plt.figure(figsize=(12, 6))

plt.plot(
    years, sales,
    color='blue', marker='o',
    label='Actual Sales Volume'
)

plt.plot(
    years, linear_trend,
    color='black', linestyle='--',
    label='Linear Trend'
)

plt.plot(
    years, poly_trend,
    color='red', marker='o',
    label='Polynomial Trend (Degree 2)'
)

plt.title('BMW Car Sales: Linear and Polynomial Trend')
plt.xlabel('Year')
plt.ylabel('Total Sales Volume')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

# Display final results
print("\nFinal Results:")
print(resampled_data)
```

### OUTPUT
<img width="777" height="636" alt="image" src="https://github.com/user-attachments/assets/d4602ca7-df03-476f-a6c1-b20f697a726c" />
<img width="323" height="193" alt="image" src="https://github.com/user-attachments/assets/7676dbf9-ec78-4835-a376-094ed774f116" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
