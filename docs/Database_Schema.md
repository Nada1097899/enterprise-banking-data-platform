# Enterprise Banking Database Schema

## Overview

This document defines the logical database schema for the Enterprise Banking Data Engineering Platform.

The schema serves as the foundation for:

- Dataset Generator
- Data Quality Framework
- ETL Pipeline
- Data Warehouse
- Star Schema
- Analytics Projects
---

# Tables

## 1. Customers

| Column Name | Data Type | Key | Description |
|-------------|-----------|-----|-------------|
| customer_id | INT | PK | Unique customer identifier |
| first_name | VARCHAR(50) | | Customer first name |
| last_name | VARCHAR(50) | | Customer last name |
| gender | VARCHAR(10) | | Gender |
| date_of_birth | DATE | | Date of birth |
| email | VARCHAR(100) | | Email address |
| phone | VARCHAR(20) | | Phone number |
| national_id | VARCHAR(20) | | National ID |
| city | VARCHAR(50) | | City |
| country | VARCHAR(50) | | Country |
| occupation | VARCHAR(100) | | Customer occupation |
| annual_income | DECIMAL(18,2) | | Annual income |
| risk_segment | VARCHAR(20) | | Customer risk category |
| created_at | DATETIME | | Record creation date |
| updated_at | DATETIME | | Last update date |
## 2. Accounts

| Column Name | Data Type | Key | Description |
|-------------|-----------|-----|-------------|
| account_id | INT | PK | Unique account identifier |
| customer_id | INT | FK | References Customers.customer_id |
| branch_id | INT | FK | References Branches.branch_id |
| account_number | VARCHAR(30) | | Unique bank account number |
| account_type | VARCHAR(30) | | Savings, Current, Salary, etc. |
| currency | CHAR(3) | | ISO Currency Code (EGP, USD...) |
| balance | DECIMAL(18,2) | | Current account balance |
| status | VARCHAR(20) | | Active, Dormant, Closed |
| opened_date | DATE | | Account opening date |
| closed_date | DATE | | Account closing date (if applicable) |
| created_at | DATETIME | | Record creation date |
| updated_at | DATETIME | | Last update date |
## 3. Transactions

| Column Name | Data Type | Key | Description |
|-------------|-----------|-----|-------------|
| transaction_id | BIGINT | PK | Unique transaction identifier |
| account_id | INT | FK | References Accounts.account_id |
| transaction_type | VARCHAR(30) | | Deposit, Withdrawal, Transfer, Payment |
| amount | DECIMAL(18,2) | | Transaction amount |
| currency | CHAR(3) | | Transaction currency |
| transaction_date | DATETIME | | Transaction date and time |
| channel | VARCHAR(30) | | ATM, Mobile App, Branch, POS, Online |
| merchant_category | VARCHAR(50) | | Merchant category |
| description | VARCHAR(255) | | Transaction description |
| status | VARCHAR(20) | | Success, Failed, Pending |
| created_at | DATETIME | | Record creation date |
| updated_at | DATETIME | | Last update date |
## 4. Branches

| Column Name | Data Type | Key | Description |
|-------------|-----------|-----|-------------|
| branch_id | INT | PK | Unique branch identifier |
| branch_code | VARCHAR(10) | | Unique branch code |
| branch_name | VARCHAR(100) | | Branch name |
| city | VARCHAR(50) | | Branch city |
| region | VARCHAR(50) | | Branch region |
| manager_name | VARCHAR(100) | | Branch manager |
| phone | VARCHAR(20) | | Branch phone number |
| opened_date | DATE | | Branch opening date |
| status | VARCHAR(20) | | Active, Closed |
| created_at | DATETIME | | Record creation date |
| updated_at | DATETIME | | Last update date |
## 5. Loans

| Column Name | Data Type | Key | Description |
|-------------|-----------|-----|-------------|
| loan_id | INT | PK | Unique loan identifier |
| customer_id | INT | FK | References Customers.customer_id |
| branch_id | INT | FK | References Branches.branch_id |
| loan_type | VARCHAR(30) | | Personal, Mortgage, Auto, Business |
| principal_amount | DECIMAL(18,2) | | Original loan amount |
| interest_rate | DECIMAL(5,2) | | Annual interest rate |
| loan_term_months | INT | | Loan duration in months |
| monthly_installment | DECIMAL(18,2) | | Monthly payment amount |
| remaining_balance | DECIMAL(18,2) | | Outstanding loan balance |
| loan_status | VARCHAR(20) | | Active, Closed, Defaulted |
| start_date | DATE | | Loan start date |
| end_date | DATE | | Loan end date |
| created_at | DATETIME | | Record creation date |
| updated_at | DATETIME | | Last update date |
## 6. Cards

| Column Name | Data Type | Key | Description |
|-------------|-----------|-----|-------------|
| card_id | INT | PK | Unique card identifier |
| account_id | INT | FK | References Accounts.account_id |
| card_number | VARCHAR(25) | | Masked card number |
| card_type | VARCHAR(20) | | Debit, Credit, Prepaid |
| network | VARCHAR(20) | | Visa, Mastercard |
| expiry_date | DATE | | Card expiry date |
| credit_limit | DECIMAL(18,2) | | Credit limit (Credit cards only) |
| available_limit | DECIMAL(18,2) | | Available credit |
| card_status | VARCHAR(20) | | Active, Blocked, Expired |
| issued_date | DATE | | Card issue date |
| created_at | DATETIME | | Record creation date |
| updated_at | DATETIME | | Last update date |
## 7. Campaigns

| Column Name | Data Type | Key | Description |
|-------------|-----------|-----|-------------|
| campaign_id | INT | PK | Unique campaign identifier |
| campaign_name | VARCHAR(100) | | Campaign name |
| channel | VARCHAR(30) | | Email, SMS, Social Media, Branch |
| target_segment | VARCHAR(50) | | Target customer segment |
| budget | DECIMAL(18,2) | | Campaign budget |
| start_date | DATE | | Campaign start date |
| end_date | DATE | | Campaign end date |
| campaign_status | VARCHAR(20) | | Planned, Active, Completed |
| created_at | DATETIME | | Record creation date |
| updated_at | DATETIME | | Last update date |
## 8. Customer_Campaigns

| Column Name | Data Type | Key | Description |
|-------------|-----------|-----|-------------|
| customer_campaign_id | BIGINT | PK | Unique record identifier |
| customer_id | INT | FK | References Customers.customer_id |
| campaign_id | INT | FK | References Campaigns.campaign_id |
| contact_date | DATETIME | | Date customer was contacted |
| channel | VARCHAR(30) | | Email, SMS, Branch, Social Media |
| response_status | VARCHAR(20) | | Sent, Opened, Clicked, Converted, Rejected |
| conversion_flag | BIT | | Indicates whether the customer converted |
| created_at | DATETIME | | Record creation date |
| updated_at | DATETIME | | Last update date |