# project1_group2
Output of the project 1 - group 2 (ahmed, jasmeen, muskan, nazim)

Question 4 : COVID correlation between vaccination compaign and Deaths 

OUTPUT : Question 4 : How many covid related deaths were reported throughout the pandemic and is there a correlation to the vaccine rollout?


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

