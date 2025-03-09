# Exploratory Data Analysis of Customer Purchase Data with Pandas
Exploratory Data Analysis (EDA) project using pandas to analyze customer purchase data, identify key trends, and extract insights on demographics, spending behaviors, professions, and more.

## **Data Overview and Structure**

### **Total Entries and Columns**

```jupyterpython
import pandas as pd

# Loading dataset
df = pd.read_csv('data/Cust_Purch_FakeData.csv')

# Getting the number of rows and columns
num_entries, num_columns = df.shape
print(f'Total Entries: {num_entries}, Total Columns: {num_columns}')
```
**Output:**
```jupyterpython
Total Entries: 30000, Total Columns: 20
```
## **Customer Demographics**

### **Customer Age Analysis**
```jupyterpython
# Finding max, min, and mean age
max_age = df['age'].max()
min_age = df['age'].min()
mean_age = df['age'].mean()

print(f'Min Age: {min_age}, Max Age: {max_age}, Mean Age: {mean_age}')
```
**Output:**
```jupyterpython
Min Age: 18, Max Age: 65, Mean Age: 41.550066666666666
```
### **Customer Name Frequency**

```jupyterpython
# Combining first and last name to get full name
df['full_name'] = df['first'] + ' ' + df['last']

# Finding the three most common customer names
top_3_names = df['full_name'].value_counts().head(3)
print('Top 3 Most Common Customer Names:')
print(top_3_names)
```
**Output:**
```jupyterpython
Top 3 Most Common Customer Names: 
Henrietta Luna    4
Leona Ruiz        3
Katie McKinney    3
Name: count, dtype: int64
```
## **Customer Identification and Contact Information**

### **Duplicate Phone Numbers**

```jupyterpython
# Finding customers with duplicate phone numbers
duplicate_phone = df[df.duplicated(subset='phone', keep=False)]
print('Customers with Duplicate Phone Numbers:')
print(duplicate_phone[['full_name', 'phone']])
```
**Output:**
```jupyterpython
Customers with Duplicate Phone Numbers:
      full_name           phone
15  Lilly Tyler  (263) 382-8004
16   Peter Cain  (263) 382-8004
```
### **Email Associations**

```jupyterpython
# Ensuring the cc_no column is treated as a string for accurate comparison
df['cc_no'] = df['cc_no'].astype(str)

# Finding the rows with the given credit card number and return the associated emails
card_email_details = df[df['cc_no'] == '5020000000000230']['email']

# Showing the count and the corresponding emails
card_email_count = card_email_details.shape[0]
print(f'Number of emails associated with credit card 5020000000000230: {card_email_count}')
print(card_email_details)
```
**Output:**
```jupyterpython
Number of emails associated with credit card 5020000000000230: 2
0    sebvajom@kol.km
1      acu@jatsot.ug
Name: email, dtype: object
```
### **Email Providers**

```jupyterpython
# Extracting email providers from email column
df['EmailProvider'] = df['email'].apply(lambda x: x.split('@')[-1])
top_5_email_providers = df['EmailProvider'].value_counts().head(5)
print('Top 5 Most Popular Email Providers:')
print(top_5_email_providers)
```
**Output:**
```jupyterpython
Top 5 Most Popular Email Providers:
EmailProvider
gmail.com      1687
me.com         1676
outlook.com    1664
live.com       1660
hotmail.com    1659
Name: count, dtype: int64
```
### **Is there any customer using email with "am.edu"?**

```jupyterpython
# Checking if any customer has an email ending with "am.edu"
am_edu_email = df[df['email'].str.contains('am.edu')]
print('Customers with "am.edu" email:')
print(am_edu_email[['full_name', 'email']])
```
**Output:**
```jupyterpython
Customers with "am.edu" email:
            full_name         email
150  Loretta Fletcher  barit@am.edu
```
## **Customer Professions**

### **Structural Engineer Count**

```jupyterpython
# Finding how many customers are Structural Engineers
structural_engineer_count = df[df['profession'] == 'Structural Engineer'].shape[0]
print(f'Number of Structural Engineers: {structural_engineer_count}')
```
**Output:**
```jupyterpython
Number of Structural Engineers: 87
```
### **Male Structural Engineers**

```jupyterpython
# Finding how many male customers are Structural Engineers
male_structural_engineers = df[(df['profession'] == 'Structural Engineer') & (df['gender'] == 'Male')].shape[0]
print(f'Number of Male Structural Engineers: {male_structural_engineers}')
```
**Output:**
```jupyterpython
Number of Male Structural Engineers: 43
```
### **Female Structural Engineers from Alberta**

```jupyterpython
# Finding female Structural Engineers from Alberta
female_structural_alberta = df[(df['profession'] == 'Structural Engineer') & (df['gender'] == 'Female') & (df['province'] == 'AB')].shape[0]
print(f'Number of Female Structural Engineers from Alberta: {female_structural_alberta}')
```
**Output:**
```jupyterpython
Number of Female Structural Engineers from Alberta: 4
```
### **Two Most Common Professions**

```jupyterpython
# Finding the two most common professions
top_2_professions = df['profession'].value_counts().head(2)
print('Top 2 Most Common Professions:')
print(top_2_professions)
```
**Output:**
```jupyterpython
Top 2 Most Common Professions:
profession
Preschool Teacher       112
Distribution Manager    107
Name: count, dtype: int64
```
## **Spending Behavior Analysis**

### **Spending Summary**

```jupyterpython
# Finding max, min, and average spending
max_spending = df['price(CAD)'].max()
min_spending = df['price(CAD)'].min()
avg_spending = df['price(CAD)'].mean()

print(f'Min Spending: {min_spending}, Average Spending: {avg_spending}, Max Spending: {max_spending}')
```
**Output:**
```jupyterpython
Min Spending: 0.0, Average Spending: 49.990775, Max Spending: 100.0
```
### **Non-Spenders**

```jupyterpython
# Finding customers who did not spend anything
non_spenders = df[df['price(CAD)'] == 0]
print('Customers who did not spend anything:')
print(non_spenders[['full_name', 'price(CAD)']])
```
**Output:**
```jupyterpython
Customers who did not spend anything:
         full_name  price(CAD)
5320   Bruce Bryan         0.0
10597  Flora Clark         0.0
```
### **Loyalty Reward Eligibility**

```jupyterpython
# Finding customers who spent 100 CAD or more
loyalty_rewards = df[df['price(CAD)'] >= 100]
print('Customers eligible for loyalty rewards (spent 100 CAD or more):')
print(loyalty_rewards[['full_name', 'price(CAD)']])
```
**Output:**
```jupyterpython
Customers eligible for loyalty rewards (spent 100 CAD or more):
              full_name  price(CAD)
76        Gregory Brown       100.0
21093  Cody Christensen       100.0
24385      Lizzie Dixon       100.0
```
### **Visa Spending**

```jupyterpython
# Finding the customer who spent 100 CAD using Visa
visa_spending = df[(df['cc_type'] == 'Visa') & (df['price(CAD)'] >= 100)]
print('Customer who spent 100 CAD using Visa:')
print(visa_spending[['full_name', 'price(CAD)', 'cc_type']])
```
**Output:**
```jupyterpython
Customer who spent 100 CAD using Visa:
        full_name  price(CAD) cc_type
76  Gregory Brown       100.0    Visa
```
## **Credit Card Information**

### **Expiring Cards**

```jupyterpython
# Converting expiration date to datetime format
df['cc_exp'] = pd.to_datetime(df['cc_exp'], errors='coerce')

# Extracting the year of expiration
df['cc_exp_year'] = df['cc_exp'].dt.year

# Finding how many cards are expiring in 2019
expiring_cards_2019 = df[df['cc_exp_year'] == 2019].shape[0]
print(f'Number of cards expiring in 2019: {expiring_cards_2019}')
```
**Output:**
```jupyterpython
Number of cards expiring in 2019: 2684
```
### **Credit Card Provider Analysis**

```jupyterpython
# Counting how many people use Visa as their credit card provider
visa_count = df[df['cc_type'] == 'Visa'].shape[0]
print(f'Number of people using Visa: {visa_count}')
```
**Output:**
```jupyterpython
Number of people using Visa: 1721
```
## **Customer Traffic and Engagement**

### **Peak Customer Day**

```jupyterpython
# Finding which day of the week the store gets the most customers
df['date'] = pd.to_datetime(df['date'], errors='coerce')
df['weekday'] = df['date'].dt.day_name()

top_day = df['weekday'].value_counts().idxmax()
print(f'The day of the week with the most customers: {top_day}')
```
**Output:**
```jupyterpython
The day of the week with the most customers: Tuesday
```


