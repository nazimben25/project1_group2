# project1_group2
Output of the project 1 - group 2 (ahmed, jasmeen, muskan, )



Project 1 – Group 2
1.	Project title
Global Investment Trends in Stock Markets During the COVID-19 Pandemic: 

2.	Team Members
-	Mansour, Ahmad
-	Narwal, Muskan
-	Kaur, Jasleen
-	Bendjaballah, Nazim

3.	Project description
Assessing correlations between vaccine rollout, stock performance and larger impact on countries.
Objective 1: Analyzing correlation between vaccine rollout, stock performance (each vaccine) and larger impact on countries
Objective 2: Global Vaccine Rollout and its Impact on Stock Markets, Economies, and Public Health"


4.	Research Questions to answer

4.1.	Which Pharmaceutical Company(Vaccine) stock performed the most during COVID-19?
    (https://github.com/nazimben25/project1_group2/tree/main/Muskan)

Question 1 : COVID -19 ( Stock Performance)-"Which Company  stock performed the most during COVID-19?"

OUTPUT : Moderna emerged as the best-performing stock overall, achieving the highest price increase and capturing investor attention with its innovative mRNA vaccine

0) 
    0.1) directory "muskan" structure 
        - stock_analysis.ipynb : contains the code to collect clean and analyse the data
        - gitignore file
        - folder Resources : with raw data (csv files)
        - folder output : with outputs of the code : dataframes under cvs, png files
        - other files : include Word and xls  documents for the projetc follow up

    0.2 ) module importation

        import pandas as pd
        import pathlib as path
        import yfinance as yf
        import matplotlib.pyplot as plt
    
       

    0.3) define a function calculate_base_100
        - function to calculate base 100 for the stocks
        - generate base 100- equation
        - print the updated Dataframe - with a new column for base-100 


1) COLLECT THE DATA

    1.1) Vaccine- Stock Data

        collected stock market data related to vaccine companies using the open-source library yfinance, which provides access to market data from Yahoo! Finance's API.

        --Extracting Data using yfinance library of Python -
    
            Define the ticker for Pfizer
            ticker = 'PFE'  # Pfizer
            Fetch data for Pfizer from yfinance
            start_date = '2019-01-01'
            end_date = '2023-12-31'
            Download the data for Pfizer
            pfizer_data = yf.download(ticker, start=start_date, end=end_date)
            Add a Ticker column
            pfizer_data['Ticker'] = ticker
            Reset index to make Date a column
            pfizer_data.reset_index(inplace=True)
            Save the data to a CSV file
            pfizer_data.to_csv('Resources/PFE_data.csv', index=False)
            Display a summary of the data
            print(pfizer_data.head())


            -merging:
                A csv file -containing the data for all vaccines 

            - cleaning : 
                 - create DataFrame : combined_df
                 - check for missing values drop duplicate columns
                 - Check the format for -Date column
                 - Identify potential Outliers
                 - drop NAN rows


2) PROCESS THE DATA
 transformation and analyses

    2.1) Calculate Yearly Change in Stocks Performace for each Vaccine(2019-2023)
        -groupby'Ticker'
        -reset index
        -Calculate-Percenatge change
        -new Column added percentage change
        -function to calculate base 100
        -set change column to base 100
        -Rename date to Year-For plotting -easy to handle 

        Output: '(Output/Q1_Performance of Vaccine stocks over time')


    2.2)Calculate Total stocks Volume for each Ticker

        -Groupby ('Ticker,'Volume)
        -Bar plot for Volume for each Vaccine
        -Output:'(Output/Q1_Total Volume of Stocks per vaccine ')


    2.3)Plot a multibar chart to -Total Volume for each ticker by year

        -Pivot the Dataframe
        -Output: ('Output/Q1_Total_Volume_Change For Each Ticker by year.png')


4.2.	QUESTION 2 - STUDYING THE CORRELATION BETWEEN VACCINE ROLLOUT OF INDIVIDUAL MANUFACTURERS AND THE STOCK VOLUME OF THEIR COMPANIES.
       (https://github.com/nazimben25/project1_group2/tree/main/Ahmad-project)

Output - linear regression graph and associated r-value allowing us to draw a conclusion on whether or not there is in fact a strong correlation between the two. In other words, whether or not it can be said statistically that as number of vaccines purchased by governments from a particular manufacturer increases, so does the stock volume of the company.

0) 
    0.1) structure of Ahmad-project folder
    -Resources folder containing vaccinations-by-manufacturers.csv dataset collected from Our World in Data covid-19-data Github directory containing covid-19 vaccine rollout per manufacturer, csv dataset output_countries_list_UN_referential.csv containing list of countries grouped by region, and csv dataset on stock volume of individual companies provided by teammate Muskan.
    -vaccine-data.ipynb code notebook.
    -Output folder containing the generated csv datasets used for graphs and all of the figures.

    0.2) module importation

        import pandas as pd
        from pathlib import Path
        import numpy as np
        import matplotlib.pyplot as plt
        import time
        import scipy.stats as st
        from scipy.stats import linregress
        
1) Data Collection

        Data was collected from Our World in Data (owid) covid-19-data Github repository, specifically the file vaccinations-by-manufacturers.csv in the vaccinations folder - https://github.com/owid/covid-19-data.git

        The Stock Volume dataset was generated via a particular code provided by my teammate Muskan, highlighted in section 2.2

2) Data processing and cleaning

    2.1) Data was loaded as per the following path:

    2.2) Cleaning the vaccinations dataset: the last column was renamed for a more visually appealing table - manufacturer_data_cleaned = manufacturer_data.rename(columns={"total_vaccinations": "Total Vaccinations per Manufacturer"})

    2.3) Cleaning the regions dataset: only the two necessary columns were kept - columns_kept = ["name", "region"]
    regions_df_cleaned = regions_df[columns_kept]
    regions_df_refined = regions_df_cleaned.rename(columns={"name": "location"}

    2.4) Once cleaned, the two datasets were merged and exported as a single csv file to use for the next step, which is visualization -
    merged_manufacturer_df = pd.merge(regions_df_refined, manufacturer_data_cleaned, on="location")
    manufacturer_path = "Output/merged_manufacturer_df.csv"
    merged_manufacturer_df.to_csv(manufacturer_path)

3) Analysis and visualizations:

    3.1) Individual bar graphs were produced for every region displaying the total vaccinations per manufacturer in that region. The graph displayed all of the manufacturers per region.
    -Firstly, a function was defined in order to only have to repear the function for each region rather than the whole code - def region_per_manufacturer_bar_chart(manufacturers_df, region_name, color='b')
    -Secondly, the maximum/final value for each manufacturer was collected for each country as the data is cumulative day by day - max_vaccinations = manufacturers_df.groupby(['location', 'region', 'vaccine'])['Total Vaccinations per Manufacturer'].max().reset_index().
    -Thirdly, the maximum values per manufacturer for each country were summed for their respective regions in order to get the totals - region_df = max_vaccinations[max_vaccinations["region"] == region_name]
    grouped_data = region_df.groupby('vaccine')['Total Vaccinations per Manufacturer'].sum().reset_index().
    -Fourthly, the data was plotted.
    -Finally, this line of code was repeated for each region, generating the bar graphs - region_per_manufacturer_bar_chart(manufacturers_df, "Latin America and Caribbean", 'b')

    3.2) A summary bar graph was generated to display the top 2 manufacturers per region in one graph as this will be what is used for the linear regression.
    -Thefirst two steps were the same as for 3.1.
    -The third step was also like 3.1 but with different code specifying only the top 2 manufacturers - top_manufacturers = region_manufacturer_data.groupby('region').apply(lambda x: x.nlargest(2, 'Total Vaccinations per Manufacturer')).reset_index(drop=True)
    -This code generated unique bar colors for each manufacturer to associate with the legend - nique_vaccines = top_manufacturers['vaccine'].unique()
    colors = plt.cm.get_cmap('tab10', len(unique_vaccines))  # Using a colormap for distinct colors
    color_mapping = {vaccine: colors(i) for i, vaccine in enumerate(unique_vaccines)}

    3.3) The code provided by my teammate Muskan allowed to get the Stock Volumes for each company.

    3.4) The top_manufacturers dataset from 3.2 was modified to have commonality with the stock volumes dataset - the company names were changed to ticker stock symbols and the "vaccine" column was renamed to "Ticker".

    3.5) The linear regression was generated, analyzing the correlation between total vaccinations of the top manufacturers and stock volumes of their companies - slope, intercept, r_value, p_value, std_err = linregress(merged_top_manufacturer_df["Total Vaccinations per Manufacturer"], merged_top_manufacturer_df["Volume"])
    regression_line = slope * merged_top_manufacturer_df["Total Vaccinations per Manufacturer"] + intercept
    line_eq = "y = " + str(round(slope,2)) + "x +" + str(round(intercept,2)).



4.3.	Is there a correlation between the vaccine rollout and the change in economy of different countries throughout this timeframe (developing, developed, transitioning government)?
    https://github.com/nazimben25/project1_group2/tree/master






























4.4.How many covid related deaths were reported throughout the pandemic and is there a correlation to the vaccine rollout?

OUTPUT : 
    https://github.com/nazimben25/project1_group2/tree/main/nazim
0) 
    0.1) directory "nazim" structure 
        - q4_death_vs_vaccination.ipynb : contains the code to collect clean and analyse the data
        - gitignore file
        - folder Resources : with raw data (csv files)
        - folder output : with outputs of the code : dataframes under cvs, png files
        - other files : include Word and xls  documents for the projetc follow up

    0.1 ) module importation

        import pandas as pd
        import pathlib as path

        import numpy as np
        from scipy.stats import linregress

        import matplotlib.pyplot as plt
        import hvplot.pandas
        import geopandas as gpd

        import requests
        import json
        from pprint import pprint

    0.2) define a function REGRESSION
        - function to calculate regression parameters
        - generate reg equation
        - generate a scatter plot + regression line for 2 series


1) COLLECT THE DATA

    1.1) Countries referential

    Official referential of Countries used by the World Bank
        will be used below to extract data (GDP + population, region, coordinates)
        can be used for different other requests
        
        1.1.1) Input
        API World Bank : https://api.worldbank.org/v2/country?format=json

        1.1.2) Ouput 
            - DataFrame : countries_df
            - csv file : Output/output_countries_list_who_referential.csv

        1.1.3) Processing
            for LOOP each page of the json file
            Extract the data related to each country : 'id', 'iso2Code', 'name', 'region','capitalCity', 'longitude', 'latitude'

    
    1.2) Collect Data for COVID deaths 

        CSV file from World Health Organization website 
        https://data.who.int/dashboards/covid19/data?n=o
        
        desciption : Daily data from 01-2020 to 09-2024 (deaths + cases by country)

        Output : deaths_df
    
    1.3) Collect Data for COVID vaccination compaign 

        csv file : KAGGLE.com
        https://www.kaggle.com/datasets/fedesoriano/coronavirus-covid19-vaccinations-data?resource=download
        
        description : data by country and per day of vaccination compaign

        Output : Vaccination_df

    1.4) Collect Data for COVID vaccination compaign End of Period

        csv file from World Health Organization website 
       https://data.who.int/dashboards/covid19/data?n=o
        
        description : data by country and per day of vaccination compaign end of period

        Output : Vaccination_eop_clean_df

    1.5) Collect total Population for each country

        1.5.1 ) Input
        API World Bank : https://api.worldbank.org/v2/country/{COUNTRY}/indicator/{INDICATOR}?date={Time Periode}&format=json

        1.5.2) Ouput 
            - DataFrame : pop_df


        1.5.3) Processing
            - for LOOP each json file (1 by country)
                extract the data related to each country 
                extraction in 2 Dataframes
                        - code Iso 3 positions
                        - code 2 positions 
                        - Total population from 2019 to 2023

            - cleaning : 
                 - create DataFrame : pop_df
                 - drop columns
                - rename columns
                - drop NAN rows


2) PROCESS THE DATA
cleaning and tranformation

    2.1) Deaths data cleaning
        - Change Datatype
        - rename columns
        - add columns (year + month)


    2.2) Deaths by country
        - groupby 'country'
        - reset_index

    2.3) deaths daily
        - groupby 'date'
        - reset_index

        - identify outliers
        - remove outliers IQR method

        - Calculate moving average to smooth the daily result


    2.4) vaccination data end of period
        - Select columns

    2.5) vaccination by country
        - vaccination_eop_clean_df

    2.6) vaccination daily
        - groupby 'date'
        - reset_index
        - change datatype

        - identify outliers
        - remove outliers IQR method
        !!! : it seems we have nested outliers> dont want to change the method and remain with IQR > i accept the dataset as it is

        - Calculate moving average to smooth the daily result

    2.7) create a DF with full data covid 
        create Df vaccination vs death vs population
        - merge 
            deaths_country_df + 
            vaccination_country_df +
            countries_df+
            pop_df
        - select columns
        - Export to csv
 




3) analysis Vaccination vs deaths

    3.1) analysis of the daily evolution
        - merge deaths_daily + vaccination daily
        - replace NAN for deaths serie (not vaccination)

        - for regression, i droped rowd where vaccination data where not available (after the compaign started)

        OUTPUT :
            - png file : plot with 2 scales
            - png file : regression + scatterplot (vaccinatin vs deaths)


    3.2) analyisis by country
        create DF
            - deaths_countries_df
            - vaccination_countries_df

        calculate : 
            - vaccination %
            - death %
        
        drop NaN
        identify and remove outliers

        OUTPUT
            - map Vaccination % by country
            - map deaths % bu country



    3.3) analysis by region
        create DF
            - groupBy "region'
        calculate : 
            - vaccination %
            - death %
        
        OUTPUT 
        png file : scatter plot by region






















5.	Plan
The first aim is to compare the performance of stocks of  major vaccine companies throughout key points of the pandemic.  In doing so, we will make an analysis of this data in order to determine which stock was the best to invest in. 

The second aim is to assess the rollout of these different vaccines per country - this data will be used to make correlations at later points. 

The third aim is to analyze whether or not there is a correlation between vaccine rollout/vaccination rates and changes in the economies of these countries : One of the stated goals of the vaccines was to help once again boost economies so this section aims to analyze whether or not there truly is a correlation there. 

Finally, we will use that same vaccine rollout data to determine if there is a correlation between vaccination rates and rates of covid-related deaths. Both analyses will feed into the predicted strength of the stocks.

6.	Possible datasets
6.1.	Aim 1 -Stocks related to vaccines - Google Finances,Yahoo Finances
6.2.	Aim 2 - Johns Hopkins University: They have been tracking COVID-19 data, including vaccination statistics, which can be accessed through their GitHub repository.
6.3.	Aim 3 - Resources from aim 2 additionally to World Bank data API
6.4.	Aim 4 -Johns Hopkins University: They have been tracking COVID-19 data, including vaccination statistics, which can be accessed through their GitHub repository.

7.	Possible Blockers and how you plan to solve them 
-	Availability of data
-	Consistency of the data between different sources

8.	A possible blocker can be identifying which countries are considered countries
We plan to solve this by having a set number of countries. 
