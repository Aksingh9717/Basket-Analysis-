# Basket-Analysis-

# Basket Analysis Project

## Problem Statement:
In a competitive retail environment, understanding customer buying patterns is essential for improving sales, cross-selling, and strategizing promotions. This project demonstrates how to perform Basket Analysis using Power BI to uncover product associations, helping businesses implement data-driven decisions.

## Project Workflow:
### 1. Understanding Basket Analysis:
Basket Analysis is an analytical technique used to uncover associations between items.
### Use Cases:

#### Product Recommendation: 
Suggest complementary products to customers.
#### Cross-Selling Strategies: 
Group products frequently bought together for promotional offers.
#### Pricing and Sales Strategies: 
Offer discounts on frequently associated items to boost revenue.

### 2. Key Concepts:
####  Support: 
The frequency of a particular item or combination in transactions.
#### Confidence: 
The likelihood of a product being bought if another product is purchased.
#### Lift: 
The strength of association between two products, relative to their individual frequencies.

### 3. Data Preparation:
#### Source: 
Import the CSV file containing transactional data into Power BI.
### Steps in Power Query Editor:
#### Load Data: Load the data into Power BI.
#### Unpivot Columns:
Add an index column (Transaction ID) to uniquely identify each transaction.

Select all product columns and unpivot them to create a transactional format.

#### Clean Data:
Remove unnecessary columns (e.g., Attributes).

Filter out blank values.

Capitalize product names for consistency.

Apply Transformation: Save and apply changes.

#### 4. Data Modeling:
Create Relationships: Ensure relationships between tables for accurate calculations.

Add Calculations:
#### Total Transactions: Count distinct transactions using DAX:
DAX

#### Total Transactions = DISTINCTCOUNT(Groceries[Transaction ID])
Product Count: Count distinct products:


#### Products Count = DISTINCTCOUNT(Groceries[Products])
### 5. Creating the Basket Analysis Table:
Generate all possible combinations of two products using DAX:


#### Basket Analysis = 
FILTER(
    CROSSJOIN(
        SELECTCOLUMNS(VALUES(Groceries[Products]), "Product1", Groceries[Products]),
        SELECTCOLUMNS(VALUES(Groceries[Products]), "Product2", Groceries[Products])
    ),
    [Product1] > [Product2]
)
#### 6. DAX Calculations for Basket Analysis:
Basket Name:

#### Basket = 'Basket Analysis'[Product1] & " - " & 'Basket Analysis'[Product2]


### Support Basket:

#### Support Basket =
VAR Prod1Transactions = 
    SELECTCOLUMNS(
        FILTER(Groceries, Groceries[Products] = 'Basket Analysis'[Product1]),
        "TransactionID", Groceries[Transaction ID]
    )

VAR Prod2Transactions = 
    SELECTCOLUMNS(
        FILTER(Groceries, Groceries[Products] = 'Basket Analysis'[Product2]),
        "TransactionID", Groceries[Transaction ID]
    )

VAR BothTransactions = INTERSECT(Prod1Transactions, Prod2Transactions)

RETURN COUNTROWS(BothTransactions) / [Total Transactions]

### Confidence of Product 1:

#### Confidence of Prod1 = 
VAR SupportProd1 = COUNTROWS(FILTER(Groceries, Groceries[Products] = 'Basket Analysis'[Product1])) / [Total Transactions]

RETURN 
'Basket Analysis'[Support Basket] / SupportProd1

### Confidence of Product 2:

#### Confidence of Prod2 = 
VAR SupportProd2 = COUNTROWS(FILTER(Groceries, Groceries[Products] = 'Basket Analysis'[Product2])) / [Total Transactions]

RETURN 'Basket Analysis'[Support Basket] / SupportProd2

### Lift:

#### Lift = 
VAR SupportProd1 = COUNTROWS(FILTER(Groceries, Groceries [Products] = 'Basket Analysis'[Product1])) / [Total Transactions]

VAR SupportProd2 = COUNTROWS(FILTER(Groceries, Groceries[Products] = 'Basket Analysis'[Product2])) / [Total Transactions]

RETURN 

'Basket Analysis'[Support Basket] / (SupportProd1 * SupportProd2)

#### Snap of Dashboard

![Screenshot 2024-11-16 170436](https://github.com/user-attachments/assets/2c21bcd8-9f85-4fc2-9894-37720e168f51)


### 7. Dashboard Design:

#### Key Components:
KPIs:

#### Total Transactions: Total unique transactions.

Products Count: Total unique products analyzed.
#### Basket Analysis Table:
Displays the combination of products with support, confidence, and lift metrics.

#### Scatter Plot:
Visualizes product relationships based on support and lift metrics.
Basket Analysis Details:

Highlights baskets with high support and lift values.

Shows individual confidence scores for each product in a pair.

### 8. Insights and Conclusions:

#### Strong Associations: 
Products like Root Vegetables - Herbs and Whipped Cream - Berries exhibit high lift and confidence, indicating strong relationships.

#### Frequent Combinations: 
Yogurt - Cream Cheese shows high support and moderate lift, suggesting frequent joint purchases.
Recommendations:

Implement cross-selling strategies for high-lift pairs.
Offer discounts on high-support pairs to increase basket value.
Use insights for product placement and bundling strategies.

#### Snap of Dashboard of yogurt with their product 

![Screenshot 2024-11-16 181946](https://github.com/user-attachments/assets/8211419a-23e1-4b31-a761-eeba39c50b18)

