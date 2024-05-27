

# H1B Visa Data Analysis Project

## Overview
This project involves analyzing H1B visa data for the year 2022 using SQL (MySQL) and Python (Pandas). The dataset consists of over 600,000 entries divided into four quarters. The data was managed using MySQL for initial handling and Google Colab for further processing.
The link to the database is: https://drive.google.com/drive/folders/1oUuL9JzzTrMCGq9O9q_dhVfTnyQKuyBs?usp=sharing

## Steps Involved

### 1. Uploading the Dataset to MySQL
The dataset was uploaded in parts using command prompt due to its large size. The steps are as follows:

```sh
cd C:\Program Files\MySQL\MySQL Server 8.0\bin
mysql -u root -p --local-infile=1
SHOW GLOBAL VARIABLES LIKE 'local_infile';
SET GLOBAL local_infile = 1;
SHOW GLOBAL VARIABLES LIKE 'local_infile';
```

### 2. Creating the Table
A table `h1b_data` was created in MySQL to store the data.

```sql
CREATE TABLE h1b_data (
    CASE_NUMBER TEXT,
    CASE_STATUS TEXT,
    RECEIVED_DATE VARCHAR(25),
    DECISION_DATE VARCHAR(25),
    ORIGINAL_CERT_DATE VARCHAR(25),
    VISA_CLASS TEXT,
    JOB_TITLE TEXT,
    SOC_CODE TEXT,
    SOC_TITLE TEXT,
    FULL_TIME_POSITION TEXT,
    BEGIN_DATE VARCHAR(25),
    END_DATE VARCHAR(25),
    TOTAL_WORKER_POSITIONS INT,
    NEW_EMPLOYMENT INT,
    CONTINUED_EMPLOYMENT INT,
    CHANGE_PREVIOUS_EMPLOYMENT INT,
    NEW_CONCURRENT_EMPLOYMENT INT,
    CHANGE_EMPLOYER INT,
    AMENDED_PETITION INT,
    EMPLOYER_NAME TEXT,
    TRADE_NAME_DBA TEXT,
    EMPLOYER_ADDRESS1 TEXT,
    EMPLOYER_ADDRESS2 TEXT,
    EMPLOYER_CITY TEXT,
    EMPLOYER_STATE TEXT,
    EMPLOYER_POSTAL_CODE TEXT,
    EMPLOYER_COUNTRY TEXT,
    EMPLOYER_PROVINCE TEXT,
    EMPLOYER_PHONE TEXT,
    EMPLOYER_PHONE_EXT FLOAT,
    NAICS_CODE INT,
    EMPLOYER_POC_LAST_NAME TEXT,
    EMPLOYER_POC_FIRST_NAME TEXT,
    EMPLOYER_POC_MIDDLE_NAME TEXT,
    EMPLOYER_POC_JOB_TITLE TEXT,
    EMPLOYER_POC_ADDRESS1 TEXT,
    EMPLOYER_POC_ADDRESS2 TEXT,
    EMPLOYER_POC_CITY TEXT,
    EMPLOYER_POC_STATE TEXT,
    EMPLOYER_POC_POSTAL_CODE TEXT,
    EMPLOYER_POC_COUNTRY TEXT,
    EMPLOYER_POC_PROVINCE TEXT,
    EMPLOYER_POC_PHONE TEXT,
    EMPLOYER_POC_PHONE_EXT FLOAT,
    EMPLOYER_POC_EMAIL TEXT,
    AGENT_REPRESENTING_EMPLOYER TEXT,
    AGENT_ATTORNEY_LAST_NAME TEXT,
    AGENT_ATTORNEY_FIRST_NAME TEXT,
    AGENT_ATTORNEY_MIDDLE_NAME TEXT,
    AGENT_ATTORNEY_ADDRESS1 TEXT,
    AGENT_ATTORNEY_ADDRESS2 TEXT,
    AGENT_ATTORNEY_CITY TEXT,
    AGENT_ATTORNEY_STATE TEXT,
    AGENT_ATTORNEY_POSTAL_CODE TEXT,
    AGENT_ATTORNEY_COUNTRY TEXT,
    AGENT_ATTORNEY_PROVINCE TEXT,
    AGENT_ATTORNEY_PHONE TEXT,
    AGENT_ATTORNEY_PHONE_EXT FLOAT,
    AGENT_ATTORNEY_EMAIL_ADDRESS TEXT,
    LAWFIRM_NAME_BUSINESS_NAME TEXT,
    STATE_OF_HIGHEST_COURT TEXT,
    NAME_OF_HIGHEST_STATE_COURT TEXT,
    WORKSITE_WORKERS INT,
    SECONDARY_ENTITY TEXT,
    SECONDARY_ENTITY_BUSINESS_NAME TEXT,
    WORKSITE_ADDRESS1 TEXT,
    WORKSITE_ADDRESS2 TEXT,
    WORKSITE_CITY TEXT,
    WORKSITE_COUNTY TEXT,
    WORKSITE_STATE TEXT,
    WORKSITE_POSTAL_CODE TEXT,
    WAGE_RATE_OF_PAY_FROM TEXT,
    WAGE_RATE_OF_PAY_TO TEXT,
    WAGE_UNIT_OF_PAY TEXT,
    PREVAILING_WAGE TEXT,
    PW_UNIT_OF_PAY TEXT,
    PW_TRACKING_NUMBER TEXT,
    PW_WAGE_LEVEL TEXT,
    PW_OES_YEAR TEXT,
    PW_OTHER_SOURCE TEXT,
    PW_OTHER_YEAR TEXT,
    PW_SURVEY_PUBLISHER TEXT,
    PW_SURVEY_NAME TEXT,
    TOTAL_WORKSITE_LOCATIONS TEXT,
    AGREE_TO_LC_STATEMENT TEXT,
    H_1B_DEPENDENT TEXT,
    WILLFUL_VIOLATOR TEXT,
    SUPPORT_H1B TEXT,
    STATUTORY_BASIS TEXT,
    APPENDIX_A_ATTACHED TEXT,
    PUBLIC_DISCLOSURE TEXT,
    PREPARER_LAST_NAME TEXT,
    PREPARER_FIRST_NAME TEXT,
    PREPARER_MIDDLE_INITIAL TEXT,
    PREPARER_BUSINESS_NAME TEXT,
    PREPARER_EMAIL TEXT
);
```

### 3. Loading the Data
Data for each quarter was loaded into the table:
"D:\UCW\Projects\SQL Capstone\LCA_Disclosure_Data_FY2022_Q1.csv"
```sh
LOAD DATA LOCAL INFILE 'D:\\UCW\\Projects\\SQL_Capstone\\LCA_Disclosure_Data_FY2022_Q1.csv' INTO TABLE h1b_data FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n' IGNORE 1 ROWS;
LOAD DATA LOCAL INFILE 'D:\\UCW\\Projects\\SQL_Capstone\\LCA_Disclosure_Data_FY2022_Q2.csv' INTO TABLE h1b_data FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n' IGNORE 1 ROWS;
LOAD DATA LOCAL INFILE 'D:\\UCW\\Projects\\SQL_Capstone\\LCA_Disclosure_Data_FY2022_Q3.csv' INTO TABLE h1b_data FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n' IGNORE 1 ROWS;
LOAD DATA LOCAL INFILE 'D:\\UCW\\Projects\\SQL_Capstone\\LCA_Disclosure_Data_FY2022_Q4.csv' INTO TABLE h1b_data FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n' IGNORE 1 ROWS;
```

### 4. Data Cleaning and Processing in MySQL
Duplicate and null values were checked and handled. The data was then exported as a CSV file.
SELECT COUNT(*), COUNT(CASE WHEN CASE_NUMBER IS NULL THEN 1 END) AS NULL_COUNT FROM h1b_data;
SELECT CASE_NUMBER, COUNT(*) FROM h1b_data GROUP BY CASE_NUMBER HAVING COUNT(*) > 1;

### 5. Data Cleaning and Processing in Google Colab
Using Google Colab and Pandas, further cleaning was done:

- Checked the count of rows:
    ```sql
    SELECT COUNT(*) FROM H1B_DATA;
    ```

- Checked data values:
    ```sql
    SELECT * FROM H1B_DATA LIMIT 5;
    ```

- Checked data types:
    ```sql
    DESCRIBE H1B_DATA;
    ```

- Converted the data type and value of the `WAGE_RATE_OF_PAY_FROM` column:
    ```sql
    UPDATE h1b_data
    SET WAGE_RATE_OF_PAY_FROM=
    CASE
        WHEN WAGE_RATE_OF_PAY_FROM= '' THEN NULL
        ELSE CAST(REPLACE(REPLACE(WAGE_RATE_OF_PAY_FROM, '$', ''), ',', '') AS DECIMAL(10,2))
    END;
    ```

- Converted dates to the correct format:
    ```sql
    UPDATE H1B_DATA SET RECEIVED_DATE = STR_TO_DATE(RECEIVED_DATE , '%m/%d/%Y');
    UPDATE H1B_DATA SET DECISION_DATE = STR_TO_DATE(DECISION_DATE , '%m/%d/%Y');
    UPDATE H1B_DATA SET BEGIN_DATE = STR_TO_DATE(BEGIN_DATE , '%m/%d/%Y');
    UPDATE H1B_DATA SET END_DATE = STR_TO_DATE(END_DATE , '%m/%d/%Y');
    ```
### 6. Running SQL Queries for Insights
Several queries were run on the dataset to extract meaningful information:

- Identified top employers in a certain state and industry:
    ```sql
    SELECT EMPLOYER_NAME, COUNT(*) AS TotalCases
    FROM h1b_data
    WHERE EMPLOYER_STATE = 'CA'  -- Replace with the desired state
    AND SOC_TITLE = 'Computer Programmers'  -- Replace with the desired industry name 
    GROUP BY EMPLOYER_NAME
    ORDER BY TotalCases DESC;
    ```

- Determined which job titles paid the most:
    ```sql
    SELECT JOB_TITLE, MAX(WAGE_RATE_OF_PAY_FROM) AS MaxWage
    FROM h1b_data
    GROUP BY JOB_TITLE
    ORDER BY MaxWage DESC;
    ```

- Analyzed job concentrations in different geographical areas:
    ```sql
    SELECT WORKSITE_STATE, JOB_TITLE, COUNT(*) AS JobCount
    FROM h1b_data 
    GROUP BY WORKSITE_STATE, JOB_TITLE
    ORDER BY JobCount DESC;
    ```

