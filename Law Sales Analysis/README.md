# Data Generation

# ChatGPT Generated Dataset
Generate python script to generate a 1000 records for Lawfirm. Including details like:

1. Invoice ID
2. Client Name
3. Service Type (e.g., Litigation, Contract Review, Mediation)
4. Case Type (e.g., Corporate Law, Family Law)
5. Invoice Date
6. Hours Billed
7. Rate per Hour
8. Total Fees
9. Payment Status (Paid, Pending, Overdue)

import pandas as pd
import numpy as np
import random
from faker import Faker
import matplotlib.pyplot as plt

fake = Faker()

# Define parameters
num_records = 1000
service_types = ['Litigation', 'Contract Review', 'Mediation', 'Consultation', 'Estate Planning', 'Legal Advisory']
case_types = ['Corporate Law', 'Family Law', 'Criminal Defense', 'Employment Law', 'Business Law', 'Probate Law']
payment_statuses = ['Paid', 'Pending', 'Overdue']

# Generate data
data = []
for i in range(num_records):
    invoice_id = 1000 + i
    client_name = fake.name()
    service_type = random.choice(service_types)
    case_type = random.choice(case_types)
    invoice_date = fake.date_between(start_date='-1y', end_date='today')
    hours_billed = round(random.uniform(1, 10), 1)
    rate_per_hour = random.choice([250, 300, 350, 400])
    total_fees = round(hours_billed * rate_per_hour, 2)
    payment_status = random.choices(payment_statuses, weights=[0.7, 0.2, 0.1])[0]  # Higher chance of 'Paid'
    
    data.append([invoice_id, client_name, service_type, case_type, invoice_date, hours_billed, rate_per_hour, total_fees, payment_status])

# Python Script

# Create DataFrame
df = pd.DataFrame(data, columns=['Invoice ID', 'Client Name', 'Service Type', 'Case Type', 'Invoice Date', 'Hours Billed', 'Rate per Hour', 'Total Fees', 'Payment Status'])

# Save to CSV
df.to_csv("law_firm_sales.csv", index=False)

# Total Revenue Calculation
total_revenue = df['Total Fees'].sum()
print(f"Total Revenue: ${total_revenue:.2f}")

# Revenue by Service Type
revenue_by_service = df.groupby('Service Type')['Total Fees'].sum()
print("\nRevenue by Service Type:")
print(revenue_by_service)

# Payment Status Distribution
payment_status_counts = df['Payment Status'].value_counts()
print("\nPayment Status Distribution:")
print(payment_status_counts)

# Average Billing Rate & Hours
avg_rate = df['Rate per Hour'].mean()
avg_hours = df['Hours Billed'].mean()
print(f"\nAverage Rate per Hour: ${avg_rate:.2f}")
print(f"Average Hours Billed per Case: {avg_hours:.2f}")

# Plot Revenue by Service Type
plt.figure(figsize=(10, 5))
revenue_by_service.plot(kind='bar', color='skyblue', edgecolor='black')
plt.title('Revenue by Service Type')
plt.ylabel('Total Fees ($)')
plt.xticks(rotation=45)
plt.show()

# Plot Payment Status Distribution
plt.figure(figsize=(6, 4))
payment_status_counts.plot(kind='pie', autopct='%1.1f%%', colors=['green', 'red', 'orange'], startangle=140)
plt.title('Payment Status Distribution')
plt.ylabel('')
plt.show()
