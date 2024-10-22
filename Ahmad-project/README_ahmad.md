# project1_group2
QUESTION 2 - STUDYING THE CORRELATION BETWEEN VACCINE ROLLOUT OF INDIVIDUAL MANUFACTURERS AND THE STOCK VOLUME OF THEIR COMPANIES.

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
