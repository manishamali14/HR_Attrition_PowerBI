# Adecco India HR Attrition Analysis (Power BI)

This is my Power BI project for the Adecco India HR Analytics case study (DACS12). The company has been losing a lot of people, especially junior staff in Sales, and the task was to go through the HR data and figure out what is going on. The case study has 20 questions (10 basic and 10 medium), and every page in the report is built around those questions and the hints that came with them.

## Files in this folder

* `Adecco_HR_Attrition.pbix` is the Power BI report
* `Adecco_HR_Analysis_Project PowerBI.xlsx` is the cleaned data the report reads from
* The DACS12 Word document is the case study with all the questions and hints

Keep the .pbix and the .xlsx in the same folder. The data is already saved inside the .pbix, so the report opens fine on its own. If you want to refresh it on a different computer, you will need to point it to the Excel file again (Transform data, then Data source settings, then Change Source).

The Google Play files in here belong to a different project.

## About the data

The dataset is the IBM HR Analytics attrition data from Kaggle. It has 1,470 employees and 35 columns, with one row per employee. There are no duplicate employee IDs and no blanks in the key columns.

## What I did in Power Query

Most of the prep is in Power Query so it runs again on every refresh:

* set the data types and trimmed and cleaned all the text columns
* added a few sanity checks (years in role, years with manager and years since promotion should never be more than years at the company). Everything passed, so I removed the check columns afterwards
* added an `Experience_Mismatch` flag for a few rows that looked odd
* added `Attrition_Flag` (1 if the person left, 0 if they stayed) so I could average it and run correlations
* turned the rating columns (1 to 4) into labels like Low, Medium, High and Very High so the charts are easier to read
* made group columns for age, tenure, income, distance and training, each with an order column so they sort properly instead of alphabetically

In DAX I added the main measures (Total Employees, Employees Left, Attrition Rate and the averages), a t test for the gender question, a `Correlations` table for the "top 3 factors" question, and a promotion band column.

## Report pages

**Easy pages (questions 1 to 10)** cover the basic questions. I used cards for the single numbers, a matrix plus a rate table wherever the hint asks for a PivotTable, and a scatter plot with a trend line for the distance question.

**Medium pages (questions 1 to 10)** cover the medium questions. Same idea: a matrix and rate table for the PivotTable questions, scatter plots with trend lines for the relationship questions, and a correlation table with a Top 3 chart for question 1.

**Department Details** is a drillthrough page. Right click any department on the other pages and choose Drill through to see that department's attrition by job role, job level, overtime and tenure, along with a list of the people who left.

## What the data says

Basic questions:

1. Overall attrition is **16.12%** (237 out of 1,470).
2. **Sales** has the highest rate at 20.63%. HR is close behind at 19.05% and R&D is at 13.84%.
3. People who left were **33.6 years** old on average, about 4 years younger than those who stayed.
4. Job satisfaction barely changes by role. It goes from 2.56 (Human Resources) to 2.79 (Healthcare Representative) out of 4.
5. Men leave at 17.01% and women at 14.80%, but the t test gives p = 0.254, so the difference is not significant.
6. Leavers earned **4,787** a month on average, roughly 30% less than people who stayed (6,833).
7. Distance from home has only a weak effect (r = 0.078). Attrition goes from about 14% for people living within 5 km to 22% for people living 21 to 29 km away.
8. Only ratings 3 and 4 are used. 84.6% of employees are rated 3 and 15.4% are rated 4.
9. **416 people (28.3%)** work overtime, and they leave at 30.5% compared with 10.4% for everyone else.
10. Average time at the company is **7.01 years** (median 5). New joiners in their first 2 years leave the most (29.8%).

Medium questions:

1. The top three factors by correlation are **TotalWorkingYears (0.171), JobLevel (0.169) and YearsInCurrentRole (0.161)**. All three are negative correlations, which basically means less experienced, junior people leave more. If you count OverTime as 1 and 0 it actually comes out on top at 0.246, but I stuck to the numeric columns like the hint suggests.
2. Job involvement matters a lot. Attrition is 33.7% at the lowest level and drops to 9.0% at the highest.
3. There is no relationship between age and job satisfaction. The trend line is flat (r = 0.005).
4. Single employees leave at 25.5%, about double the rate of married (12.5%) and divorced (10.1%) employees.
5. No training last year means the highest attrition (27.8%) and 6 trainings means the lowest (9.2%). The pattern in between goes up and down a bit though.
6. Work life balance does not change performance ratings (r = 0.003).
7. Stock options make a big difference: 24.4% attrition with none and 9.9% with any stock options. Interesting side note, all 470 employees with marital status "Single" are on level 0, which probably explains part of the result in question 4.
8. Tenure and job satisfaction are not related either (r = 0.004).
9. Business travel hardly affects satisfaction (2.70 to 2.79), but frequent travellers do leave a lot more (24.9% compared with 8.0% for people who don't travel).
10. Years since last promotion does not affect performance ratings (r = 0.018).

## My takeaway

The people most likely to leave are young, junior, lower paid, working overtime, without stock options and in their first couple of years. Sales staff who work overtime are the worst group at 37.5%. Satisfaction scores are pretty much the same everywhere, so the problem looks more like pay, workload and career growth than people being unhappy with the job itself.

If I were presenting this to HR, I would start with overtime in Sales, stock options (at least level 1) for junior staff, and better support for people in their first two years.

## Things to keep in mind

* Performance ratings only use 3 and 4, so they can't show much difference between people.
* Correlation isn't causation. These are patterns in the data, not proof of why people left.

## Tools

Power BI Desktop (Power Query and DAX), with Excel used for the initial clean up.
