# Tobacco Control Compliance Monitoring

## 📚 Table of contents
1. [Project Overview](#project-overview)
2. [Tools & Technologies](#tools--technologies)
3. [Dataset overview](#dataset-overview)
4. [Data cleaning & preparation](#data-cleaning--preparation)
5. [Exploratory Data Analysis](#exploratory-data-analysis)
6. [PowerBi Dashboard](#powerbi-dashboard)
7. [Key Insights](#key-insights)
8. [Recommendations](#recommendations)
9. [Data Source](#data-source)

### Project Overview
This project seeks to evaluate the knowledge and compliance of different recreational facilities on the sale, use, and control of Tobacco and Tobacco products across six states in the six geopolitical zones in Nigeria. 
Data was collected using Kobo Collect, a digital survey ODK tool for data collection, and was exported into an Excel spreadsheet for data cleaning and preparation.
The data was cleaned by filling in missing values, taking out incorrect values, and ensuring all column values are in the correct data type formats.
Data was prepared, and a preliminary analysis was carried out by calculating the required measures and columns not originally contained in the dataset before exploratory data analysis (EDA) commenced. EDA was designed to reveal hidden trends and patterns, correlations, relationships, and associations between variables in the dataset so useful insights could be drawn. 

### Tools & Technologies
The following tools were used in this project
1. Kobo Collect: Used for data collection
2. Excel: Used for data cleaning and preparation
3. MySQL: Used for preliminary Analysis and EDA
4. Jupyter Notebook: Used to document the EDA process & codes
5. PowerBI: Used to visualize trends, patterns, and Key Metrics

### Dataset Overview

Sample Preview (Table rows and columns were truncated for easier readability)
![Screenshot 2025-09-23 224213](https://github.com/user-attachments/assets/084d24c2-34a7-4496-b323-34433dab62d0)

### Data Cleaning & Preparation
* The dataset was scanned for missing or null values and handled appropriately
* Incorrect entries were corrected or taken out
* Converted numeric and string columns to proper data type formats
* Converted the date values to date format types

### Exploratory Data Analysis
#### Question 1: How many facilities were visited across the six geopolitical zones?
```sql
SELECT table_name,
       table_rows AS estimated_rows
FROM information_schema.tables
WHERE table_schema = 'tobacco_compliance_monitoring'
  AND table_type = 'BASE TABLE'
UNION ALL
SELECT 'TOTAL',
       SUM(table_rows)
FROM information_schema.tables
WHERE table_schema = 'tobacco_compliance_monitoring'
  AND table_type = 'BASE TABLE';
```

#### Question 2: How many facilities in all six states know about the tobacco control laws?
```sql
SELECT 'Lagos' AS State, COUNT(*) AS Number_Aware
FROM Lagos
WHERE `Awareness of tobacco control law` = 'YES'

UNION ALL
SELECT 'Enugu' AS State, COUNT(*) AS Number_Aware
FROM Enugu
WHERE `Awareness of tobacco control law` = 'YES'

UNION ALL
SELECT 'Fct' AS State, COUNT(*) AS Number_Aware
FROM Fct
WHERE `Awareness of tobacco control law` = 'YES'

UNION ALL
SELECT 'Adamawa' AS State, COUNT(*) AS Number_Aware
FROM Adamawa
WHERE `Awareness of tobacco control law` = 'YES'

UNION ALL
SELECT 'Kano' AS State, COUNT(*) AS Number_Aware
FROM Kano
WHERE `Awareness of tobacco control law` = 'YES'

UNION ALL
SELECT 'Rivers' AS State, COUNT(*) AS Number_Aware
FROM Rivers
WHERE `Awareness of tobacco control law` = 'YES'

UNION ALL
-- total row
SELECT 'TOTAL' AS State, SUM(Number_Aware) AS Number_Aware
FROM (
    SELECT COUNT(*) AS Number_Aware FROM Lagos WHERE `Awareness of tobacco control law` = 'YES'
    UNION ALL
    SELECT COUNT(*) FROM Enugu WHERE `Awareness of tobacco control law` = 'YES'
    UNION ALL
    SELECT COUNT(*) FROM Fct WHERE `Awareness of tobacco control law` = 'YES'
    UNION ALL
    SELECT COUNT(*) FROM Adamawa WHERE `Awareness of tobacco control law` = 'YES'
    UNION ALL
    SELECT COUNT(*) FROM Kano WHERE `Awareness of tobacco control law` = 'YES'
    UNION ALL
    SELECT COUNT(*) FROM Rivers WHERE `Awareness of tobacco control law` = 'YES'
) AS totals
ORDER BY State ASC;
```

#### Question 3: What is the percentage of knowledge of Tobacco control laws?
```sql
SELECT 
    'Lagos' AS State,
    COUNT(*) AS Total_Facilities,
    SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) AS Number_Aware,
    ROUND((SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 0) AS Knowledge_Percent
FROM Lagos

UNION ALL
SELECT 
    'Enugu',
    COUNT(*),
    SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END),
    ROUND((SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 0)
FROM Enugu

UNION ALL
SELECT 
    'Fct',
    COUNT(*),
    SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END),
    ROUND((SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 0)
FROM Fct

UNION ALL
SELECT 
    'Adamawa',
    COUNT(*),
    SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END),
    ROUND((SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 0)
FROM Adamawa

UNION ALL
SELECT 
    'Kano',
    COUNT(*),
    SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END),
    ROUND((SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 0)
FROM Kano

UNION ALL
SELECT 
    'Rivers',
    COUNT(*),
    SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END),
    ROUND((SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 0)
FROM Rivers

--  TOTAL row across all states
UNION ALL
SELECT 
    'TOTAL',
    COUNT(*) AS Total_Facilities,
    SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) AS Number_Aware,
    ROUND((SUM(CASE WHEN `Awareness of tobacco control law` = 'YES' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 0) AS Knowledge_Percent
FROM (
    SELECT * FROM Lagos
    UNION ALL SELECT * FROM Enugu
    UNION ALL SELECT * FROM Fct
    UNION ALL SELECT * FROM Adamawa
    UNION ALL SELECT * FROM Kano
    UNION ALL SELECT * FROM Rivers
) AS all_states
ORDER BY State ASC;
```

### PowerBI Dashboard
The PowerBI Dashboard shows visuals of Key metrics at the geopolitical level. Slicers and filters were included to drill down to state, LGA, and Ward Levels as needed.
DAX expressions were used to derive Key Metrics like Percent Awareness, Compliance, and Sale.
- 📊 Shows awareness of Tobacco control laws in Nigeria across the six states
- 💹 Shows tobacco sales and compliance level per state
- 🗺️ Shows geographic location and awareness level for each state, representing each of the six geopolitical zones.
- 📑 Shows Tobacco use per facility Manager/Owner

![Screenshot 2025-09-24 225546](https://github.com/user-attachments/assets/dc32682d-aaab-4017-93d8-73fb2a47132e)

### Key Insights
* Awareness about tobacco control laws in Nigeria across the six states is less than 50%
* Compliance rate with tobacco control regulations is less than 10%
* Enugu, with the lowest awareness rate, 17% sells the most tobacco products and contributes 42% (the highest) to the 37% selling rate
* Even though Lagos has the highest awareness of TCL, 68%, it has one of the lowest compliance rates, 1%
* Facilities' knowledge of the guidelines by NTCA on the provision of Designated Smoking Areas (DSA) is poor, and just 4% of facilities across the six states have DSAs available.

### Recommendations
* More awareness is required on the control laws guiding the sale and use of tobacco and tobacco products in Nigeria
* Effective measures should be put in place to ensure facilities involved in the purchase, sale, and distribution of tobacco and tobacco products comply with regulatory laws
* Stricter sanctions can be enforced to ensure compliance
* Facility managers need more awareness about designated smoking areas and making them more available in their facilities.

### Data Source

[Download Here](https://docs.google.com/spreadsheets/d/1TmKMWxvPtgkYpPYPhWSnb_RiZKKP16rV/edit?usp=sharing&ouid=106789614816521569298&rtpof=true&sd=true)
