![istockphoto-1456035845-1024x1024](https://github.com/user-attachments/assets/fabfd590-680e-4d63-954d-7799f0befad3)


# Health-Care-Data-Analysis
Healthcare is the most important industry for any country in the world. As the world has reshaped after Covid-19 pandemic, healthcare has become a crucial part of patients' well being. In today's ever-evolving healthcare industry, data analysis plays a necessary role in driving informed 'data driven' decisions and improving patient care.

With the vast amount of healthcare data available, SQL (Structured Query Language) is an undeniably critical tool in data analysis. This powerful tool helps to gain valuable insights.

In this report, I have assumed the role of 'healthcare data analyst' at a hospital. This analysis aims to reach the depths of healthcare data, exploring various aspects such as patient average length of stay (ALOS), medical procedures, and treatment outcomes. We can uncover patterns, trends, and correlations within the data that can inform healthcare providers, administrators, and policymakers in their efforts to enhance the quality and accessibility of healthcare services.

This analysis strives to transform data into actionable knowledge and ultimately contributing to the advancement of patient care.



**Key Insights**:


The length of stay is at least 1 day and at most 14 days. On average it is 3 days.

The most number of surgeries and procedures are performed in these departments that include Surgery-Thoracic, Surgery-Cardiovascular, Radiologist, Cardiology and Surgery-Vascular.

There is a correlation between patients staying longer in the hospital and having more lab procedures done.

There are patients who are being readmitted to the hospital within 30 days that are coming through the emergency room.

There is an increase of lab procedures in African American and Hispanic patients.

**The Data**:

This dataset represents 10 years (1999-2008) of clinical care at 130 US hospitals and integrated delivery networks. It includes over 50 features representing patient and hospital outcomes. For this project we are using a dataset from Kaggle. You can find it here Dataset.

Information was extracted from the database for encounters that satisfied the following criteria.



It is an inpatient encounter, a hospital admission.

It is a diabetic encounter, that is, one during which any kind of diabetes was entered to the system as a diagnosis.

The length of stay was at least 1 day and at most 14 days.

Laboratory tests were performed during the encounter.

Medications were administered during the encounter.



The data contains attributes such as patient number, race, gender, age, admission type, time in hospital, medical speciality of admitting physician, number of lab tests performed, test results, diagnosis, number of medications, diabetic medications, number of outpatient, inpatient, and emergency visits in the year before the hospitalisation.

**Exploration & Tools**:
I used SQL Server Management Studio 19 (SSMS) and Microsoft Power BI to uncover the objectives of this analysis.

**High level over view**: 

This is a screenshot of a Power BI dashboard, answering the below questions.

![image](https://github.com/user-attachments/assets/b067ea67-9f78-4f10-99dc-877b39c221e1)





**Questions**

1.How long do patients stay admitted in hospital?

SQL Query:

*select
distinct(count(time_in_hospital)) as count_time_spent, time_in_hospital
from diabetes_data
group by time_in_hospital
order by count_time_spent desc*

SQL Output:

![image](https://github.com/user-attachments/assets/00d9c20e-70ea-4ac5-ba8a-8124580e7205)





Power BI Viz:


![image](https://github.com/user-attachments/assets/6c37142f-7c08-45fd-aca1-b126b6e50186)




**Insight**:

Both the SQL query and Visualisation (Viz) in Power BI show us that most patients spend three(3) days in admission.

2. There is a correlation between time spent and number of procedures done. This is proved with the SQL query and Power BI Viz below.

SQL Query:

*select
case
when num_lab_procedures >=0 and num_lab_procedures < 30 then 'Few'
when num_lab_procedures >=30 and num_lab_procedures <60 then 'Average'
else 'Many'
end as frequency_of_procedures ,
round(avg(time_in_hospital), 2) avg_time
from diabetes_data
group by case
when num_lab_procedures >=0 and num_lab_procedures < 30 then 'Few'
when num_lab_procedures >=30 and num_lab_procedures <60 then 'Average'
else 'Many'
end
order by avg_time desc*

SQL Output:

![image](https://github.com/user-attachments/assets/a313315f-7995-47ea-8483-8d392a4718fa)





Power BI Viz:

![image](https://github.com/user-attachments/assets/a4bc043e-2abc-45f7-922d-fe514ce61457)





3. How is the count of readmission after 30 days?

This question seeks to give insights into how many readmission there are to the same medical facility after a thirty day period. We can see that mostly more patients are not readmitted. There less numbers of readmission under thirty days and a larger portion being readmitted after thirty days.

SQL Query:

*select
distinct(count(readmitted)) readmission_count, readmitted
from diabetes_data
group by readmitted
order by readmitted desc*

SQL Output:

![image](https://github.com/user-attachments/assets/06052bf8-cf2e-4ab8-968f-b8ac6a11efed)





4. How does the count of admission types look?

It can be seen from the output that most admission are from the emergency category.
SQL Query:
select distinct(count(admission_type_id)) counts, admission_type_id,
case
when admission_type_id in ('1') then 'Emergency' /* 53990*/
when admission_type_id in ('2') then 'Urgent' /*18480*/
when admission_type_id in ('3') then 'Elective' /*18869*/
when admission_type_id in ('4') then 'Newborn' /*10*/
when admission_type_id in ('5') then 'Not Available' /*4785*/
when admission_type_id in ('6') then 'NULL' /*5291*/
when admission_type_id in ('7') then 'Trauma Center' /*21*/
when admission_type_id in ('8') then 'Not Mapped' /*320*/
else 'Others'
end as admissions
from diabetes_data
group by admission_type_id
order by counts desc

SQL Output:

![image](https://github.com/user-attachments/assets/d6471a92-97ce-4aa7-be05-c37b7eba2012)



5. How can we classify the discharge disposition?

It can be seen from the SQL output that the bigger number of patients are discharged to their homes.

SQL Query:

select distinct(count(discharge_disposition_id)) discharges, discharge_disposition_id,
case
when discharge_disposition_id in ('1') then 'Discharged to home'
when discharge_disposition_id in ('2') then 'Discharged/transferred to another short term hospital'
when discharge_disposition_id in ('3') then 'Discharged/transferred to SNF'
when discharge_disposition_id in ('4') then 'Discharged/transferred to ICF'
when discharge_disposition_id in ('5') then 'Discharged/transferred to another type of inpatient care institution'
when discharge_disposition_id in ('6') then 'Discharged/transferred to home with home health service'
when discharge_disposition_id in ('7') then 'Left AMA'
when discharge_disposition_id in ('8') then 'Discharged/transferred to home under care of Home IV provider'
when discharge_disposition_id in ('9') then 'Admitted as an inpatient to this hospital'
when discharge_disposition_id in ('10') then 'Neonate discharged to another hospital for neonatal aftercare'
when discharge_disposition_id in ('11') then 'Expired'
when discharge_disposition_id in ('12') then 'Still patient or expected to return for outpatient services'
when discharge_disposition_id in ('13') then 'Hospice / home'
when discharge_disposition_id in ('14') then 'Hospice / medical facility'
when discharge_disposition_id in ('15') then 'Discharged/transferred within this institution to Medicare approved swing bed'
when discharge_disposition_id in ('16') then 'Discharged/transferred/referred another institution for outpatient services'
when discharge_disposition_id in ('17') then 'Discharged/transferred/referred to this institution for outpatient services'
when discharge_disposition_id in ('18') then 'NULL'
when discharge_disposition_id in ('19') then 'Expired at home. Medicaid only, hospice.'
when discharge_disposition_id in ('20') then 'Expired in a medical facility. Medicaid only, hospice.'
when discharge_disposition_id in ('21') then 'Expired, place unknown. Medicaid only, hospice.'
when discharge_disposition_id in ('22') then 'Discharged/transferred to another rehab fac including rehab units of a hospital .'
when discharge_disposition_id in ('23') then 'Discharged/transferred to a long term care hospital.'
when discharge_disposition_id in ('24') then 'Discharged/transferred to a nursing facility certified under Medicaid but not certified under Medicare.'
when discharge_disposition_id in ('25') then 'Not Mapped'
when discharge_disposition_id in ('26') then 'Unknown/Invalid'
when discharge_disposition_id in ('27') then 'Discharged/transferred to another Type of Health Care Institution not Defined Elsewhere'
when discharge_disposition_id in ('28') then 'Discharged/transferred to a federal health care facility.'
when discharge_disposition_id in ('29') then 'Discharged/transferred/referred to a psychiatric hospital of psychiatric distinct part unit of a hospital'
when discharge_disposition_id in ('30') then 'Discharged/transferred to a Critical Access Hospital (CAH).'
else 'Others'
end as discharge_disposition
from diabetes_data
group by discharge_disposition_id
order by discharges desc

SQL Output:

![image](https://github.com/user-attachments/assets/8f575f46-52e2-4955-b651-df1403ba791d)




Comment: The essence of this exercise is to show case skills/competences in Data Analysis with use of SQL as a query language and Microsoft Power BI as a business intelligence and data visualisation tool.
