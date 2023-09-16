# California_homicides
Information on homicides in California are reported by various LEAs as part of the reporting requirements for the Federal Uniform Crime Reporting (UCR) Program. These data provide detailed information about the circumstances of each homicide in addition to personal characteristics of the victim.


#### -- Project Status: [Active]

## Project Intro/Objective
I'm using this project to showcase my data & policy skills to potential employers. 

*Note: my end goal is landing a job where I can apply my skills and help address some of the challenges we face today, such as crime. That said, the data used in this project represent real human lives, and I would like to make clear I have complete respect for each and every one of the lives lost. My sincere condolences to the families impacted, and I vouch to work respectfully to draw insights that may help inform policies to protect the communities impacted by violence.

Please refer to the California OpenJustice webpage to download and for more information about the data used in this project. Here's the direct link: https://openjustice.doj.ca.gov/data.

I included the help file entitled 'Homicide Context_2018_2022_06122023.pdf' in the repository. 

I also included the master datafile entitled 'HomicideActuals1987-2022.csv' and an exel file containing NCIC codes entitled 'NCIC Code Jurisdiction List_04242023.xlsx'.

There are two main jupyter notebook files:
1) 'CA_homicide_data_preparation.ipynb': this is a working file that cleans a selected set of variables from the master homicide data file.
    
3) 'Homicide_Final_analysis_Report.ipynb': this file showcases an initial demo exploratory analysis of a subset dataset prepared in the jupyter file 1) above. The subset dataset used contains a select set of variables for years 2018 to 2022 only.


### Data Link
* California Department of Justice
* https://openjustice.doj.ca.gov/data

### Methods Used
* Data Cleaning/Preparation
* Data Exploration
* Data Visualization

### Technologies
* Python
* Pandas, jupyter
* Matplotlib


## Contact
* Feel free to contact me with any questions or if you are interested in contributing!



## Project Description
*Goal 1: clean and prepare the data for thorough analysis. This is just for demonstration purposes. Not all variables are cleaned--particularly variables that are in numerical code form rather than full text description.
*Goal 2: conduct initial data exploratory analysis for an overview of the data. 


# Explanation of Data

The above dataframe "df" reflects the data contained from the csv file 'homicide.csv', which is a subset of the 'HomicideActuals1987-2022.csv' file that was downloaded from the State of California OpenJustice portal at https://openjustice.doj.ca.gov/data on August 23, 2023 under the "Homicide" subsection. For our analysis, "df" contains data from 2018 to 2022 only, and only a selected subset of variables. Additionally, some variables were renamed for the purposes of this analysis. Lastly, missing values, duplicates, and data types were manipulated when creating the subset 'homicide.csv' file, and further manipulation will be conducted bellow in this analysis. 

## Variable Changes (Code used)

Some manipulations include:
    
    Duplicate observations were dropped on master file as
        df = df.drop_duplicates()
        
    Renaming columns from master file as
        df = df.rename(columns={'CO': 'County_Codes', 'NCIC':'NCIC_Agency_Codes',
                        'BCS': 'BCS_number', 'vict num': 'Victim_number',
                        'Rpt MO' : 'Rpt_mo', 'Rpt YR': 'Rpt_YR', 
                        'Tot vic': 'Total_victims', 'Tot susp':'Total_suspects',
                        'V sex': 'Gender', 'V race': 'Victim_Race', 'V age': 'Victim_Age', 
                        'Crm Stat': 'Crime_status','Inc MO':'Incident_Month',
                        'Inc day': 'Inc_day', 'Inc YR': 'Inc_YR', 'Week day': 'Week_day',
                        'death YR': "death_YR", 'Loc': 'Location', 'PE 1': 'Precipitating_event', "Spec Circ 1": 
                        'Spec_Circ_1'})
   
    Recoding na values per data documentation as 
       df.fillna('Value not reported', inplace=True). 
    
    Recoding the gender variable as 
        df['Gender'].replace(0, 'Unknown', inplace=True)
        df['Gender'].replace(1, 'Male', inplace=True)
        df['Gender'].replace(2, 'Female', inplace=True)
    
    Recoding the age variable as
        df['Victim_Age'].replace('BB', 'One week to 12 months', inplace=True)
        df['Victim_Age'].replace('NB', 'Birth to one week', inplace=True)
        df['Victim_Age'].replace('99', '99 or over', inplace=True)
        df['Victim_Age'].replace('0', 'Unknown', inplace=True)
    Recoding race variable as
        df['Victim_Race'].replace(['H', 'X', 'W', 'B', 'A', 'O', 'U', 'I', 'C', 'S', 'D', 'P', 'F', 'V', 'Z', 'K',
        'J', 'L', 'G'], ['Hispanic', 'Unkown', 'White', 'Black', 'Other Asian', 'Other', 'Hawaiian', 'American 
        Indian', 'Chinese','Samoan','Cambodian', 'Pacific Islander', 'Filipino', 'Vietnamese', 'Asian Indian', 
        'Korean','Japanese','Laotian', 'Guamanian'],inplace=True)
        
    Recoding crime status variable (only one value reported)
        df['Crime_status'].replace(1,'Actual/Willful Homicide' , inplace=True)
        
    Recoding Week day variable from code to full text
        df['Week_day'].replace([1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0], 
                       ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday','Friday', 'Saturday'], 
                        inplace=True)
    Date variable created as datetime dtype 
        df['date'] = pd.to_datetime(dict(year=df.Inc_YR, month=df.Incident_Month, day=df.Inc_day))

         
Original filtered (>2017) 2018-2022 file contained 10100 observations after dropping duplicates. 

*Please refer to the jupyter file titled 'CA_homicide_data_CleanPrep' for the data cleaning/preparation process. In addition, refer to the 'Homicide Context_2018_2022_06122023.pdf' for further overview of the data included in the master data file. 

In addition, to get county names as text rather than codes, the XLSX file 'Agency Name Mapping', which is also found at https://openjustice.doj.ca.gov/data unde the 'Agency Name - Jurisdiction Listing' subsection, is used. Similarly, Gender, Victim_Race, Victim_Age, Week_day, Precipitating_event, and Victim_offender_Relationship_1 codes are matched according to variable/value codes and descriptions explained in the 'Homicide Context_2018_2022_06122023.pdf'. 


##  DATA Background:

Per the 'Homicide Context_2018_2022_06122023.pdf':
Caifornia's Department of Justice (DOJ) Criminal Justice Statistics Center (CJSC) collects information on homicides reported in California by various law enforcement entities (LEA) as part of the reporting requirements for the Federal Uniform Crime Reporting (UCR) Program. These data provide detailed information about the circumstances of each homicide in addition to personal characteristics of the victim. In 2016, the FBI Director informed all state Statistical Analysis Centers that the FBI’s Uniform Crime Reporting (UCR) program would be transitioning to a National Incident-Based Reporting System (NIBRS) only data collection by January 1, 2021. The California DOJ embarked on a five year effort to develop and implement a new state repository, the California Incident-Based Reporting System (CIBRS), to house the new FBI statistical reporting format. The CIBRS repository is a combination of the federal NIBRS requirements with additional California-specific data elements. The California DOJ began collecting data in CIBRS in 2021. However, not all California law enforcement agencies (LEAs) have transitioned to the new format. The 2021-2022 homicide data are the result of a combination of incident information collected through summary reporting and incident-based (IBR) formats. As with the implementation of any new data collection system, caution should be used when comparing these data with those of previous years.

For this analysis, each victim and incident combination is examined. That is, the original master file contains entries for each incident and whether more than one victim was involved for each incident. The 'df' dataframe used in this analysis will not match all victims per incident if more than one homicide occured in an incident. Only each victim will be examined in this analysis. Furthermore, the date variable in 'df' represents the date that the assault that ultimately caused the death took place.

