# project1_group2
Output of the project 1 - group 2 (ahmed, jasmeen, muskan, nazim)

OUTPUT : Question 4 : How many covid related deaths were reported throughout the pandemic and is there a correlation to the vaccine rollout?


0) module importation

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

1) CREATE DATA FRAME FOR DEATHS STATISTICS
    
    1.1) Input  : 
    Resource/ : CSV file from World Health Organization csv file 
    https://data.who.int/dashboards/covid19/data?n=o
    desciption : Daily data from 2020 to 2024
    
    1.2) output 
        1.2.1 ) DataFrame death_total_country : Aggregation by country of 
            - Cumulative_cases 
            - Cumulative_deaths 
        
        1.2.1 ) daily deaths
            - new DataFrame death_daily_clean_df : daily death per day > mobile average 7days
            - npg export chart 

    1.3) cleaning 
    no needed cleaning for the 1.2.1

    for 1.2.2, 
        - identify outliers
        - drop the outliers from the DF and work with a death_daily_clean_df
        - calculate a mobile average (7 days basis)


2) CREATE DATAFRAME FOR VACCINATION STATISTICS
    
    2.1) Input 
    'Resources/vaccination-data.csv'
    https://data.who.int/dashboards/covid19/data?n=o
    description : data by country of vaccination compaign


    2.2) Output 
    DataFrame 'death_vaccins_df_clean '
    csv file > 'Output/output_death_vaccination_data.csv'


    2.3) processing
        - Merge Vaccination_df + death_total_country
        - cleaning : rename columns

3) Extract the official referential of Countries used by the World Bank
    ## will be used below to extract data (GDP + population)
    ## can be used for different other requests
    
    3.1) Input
    API World Bank : https://api.worldbank.org/v2/country?format=json

    3.2) Ouput 
        - DataFrame : countries_df
        - csv file : Output/output_countries_list_who_referential.csv

    3.3) Processing
    for LOOP each page of the json file
    extract the data releted to each country 

4) CREATE DataFrame WITH GDP_perCapita & POPULATION DATA FOR EACH COUNTRY
    # data from 2019 to 2023

    4.1) Input
    API World Bank : https://api.worldbank.org/v2/country/{COUNTRY}/indicator/{INDICATOR}?date={Time Periode}&format=json

    4.2) Ouput 
        - DataFrame : gdp_pop_df
        - csv file : Output/output_countries_gdp_pop.csv

    4.3) Processing
        - for LOOP each json file (1 by country)
            extract the data related to each country 
            extraction in 2 Dataframes
                    - code Iso 3 positions
                    - code 2 positions 
                    - GDP per capita from 2019 to 2023
                    - Total population from 2019 to 2023
        - Merge the 2 DF

        - cleaning : 
            - drop columns
            - rename columns
            - drop NAN rows

5) create a DF with full data covid

    5.1) Output
        - DataFrame data_covid_macro
        - csv file : 'Output/output_covid_data_macro.csv'
    
    5.2) processing 
        - MERGE death_vaccins_df_clean WITH gdp_pop_df
        - Merge the new DF with Coutries_df : to retreie coordinate for each country
        - cleaning
            - Drop useless columns

6) Analysis
    multiple analysis 
        - box plots on data : Vaccination%, death%, Growth
        - maps representing countries and scale of Vaccination%, death%, Growth
        - scatterplots and regression on data : box plots on data : Vaccination%, death%, Growth

        6.1) output
            png files in folder Output

        6.2) Processing
            - cleaning : selection of useful columns

            - calculation of new values
                - Vaccination_%
                - death_%
                - Grwth

