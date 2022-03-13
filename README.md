# PyBer_Analysis
## Overview of Project

V. Isualize has asked for a summary of the PyBer ride share data by city type (i.e. Urban, Suburban and Rural) using Pandas and Matplotlib.  Below is a discussion of the result with clips of code, clips of the summary DataFrame and a multiple-line graph showing the weekly fares for each city type.


## Results of Analysis

### Summary of Pyber Data by City Types

Provided below is a image of the Summary DF with the total number of rides and drivers per city type, sum of fares for each city type, and average fare per city type and driver.

![Summary DF](https://github.com/blueschistrocks/PyBer_Analysis/blob/c28d79ebe273023180a36ce7e6c8e4966ec691b8/analysis/pyber_summary_df.png)

Two data sets of Pyber date were merged using pd.merge. The analysis of the data sets to prepared the Pybre data summary was conducted using the code provided below.

### Code
     #Combine the data into a single dataset
     pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])

     #Display the data table for preview
     pyber_data_df.head()

     #Total rides for each city type
     pyber_data_df = pyber_data_df.set_index(["type"])
     ride_count = pyber_data_df.groupby(["type"]).count()["ride_id"]

     #Total drivers for each city type
     drivers_sum_type = city_data_df.groupby(['type']).sum()['driver_count']
     drivers_sum_total = drivers_sum_type.sum()

     #Total amount of fares for each city type
     fare_sum = pyber_data_df.groupby(["type"]).sum()["fare"]

     #Average fare per ride for each city type. 
     Ave_Fare = pyber_data_df.groupby(["type"]).mean()["fare"]

     #Average fare per driver for each city type. 
     average_fare_dr_type = fare_sum.div(drivers_sum_type);

     #PyBer summary DataFrame. 
     pyber_summary_df  = pd.concat([ride_count, drivers_sum_type, fare_sum, Ave_Fare, average_fare_dr_type ], axis=1).reset_index()
     pyber_summary_df.set_index("type", inplace = True)
     pyber_summary_df.columns = ["Total Rides", "Total Drivers", "Total Fares","Average Fare Per Ride", "Average Fare Per Driver"]
     pyber_summary_df

     #Cleaned up the DataFrame. Deleted the index name.
     pyber_summary_df.index.name = None	
     #Formatted the columns
     pyber_summary_df["Total Fares"] = pyber_summary_df["Total Fares"].map("${:,.2f}".format)
     pyber_summary_df["Average Fare Per Ride"] = pyber_summary_df["Average Fare Per Ride"].map("${:,.2f}".format)
     pyber_summary_df["Average Fare Per Driver"] = pyber_summary_df["Average Fare Per Driver"].map("${:,.2f}".format)
     pyber_summary_df

### Creation of a Multiple Line Plot

Provided below is a image of the multiple line plot showing the total fares between the beginning of January 2019 and the end of April 2019.

![plot](https://github.com/blueschistrocks/PyBer_Analysis/blob/c28d79ebe273023180a36ce7e6c8e4966ec691b8/analysis/fare_summary_byType.png)

The merged DataFrame used in the data summary above was used to create the multiline plot.

### Examples of Code
	
    #Groupby() was used to create a new DataFrame showing the sum of the fares.
    pyber_data_df2 = pyber_data_df2.set_index(['type' , 'date'])
    fare_sum_type_DF = pyber_data_df2.groupby(['type', 'date'])["fare"].sum()
    fare_sum_type_DF

    #The index of the DataFrame created above was reset to used the pivot() function.
    fare_sum_type_DF = fare_sum_type_DF.reset_index()
    fare_sum_type_DF

     #A pivot table was created with the date as index and column the city types.
     fare_sum_type_DF_pv = fare_sum_type_DF.pivot(index='date', columns='type', values='fare')
    fare_sum_type_DF_pv

     #Using loc a new DataFram was created with the dates that contained '2019-01-01':'2019-04-29'
     fare_sum_type_DF_pv_loc = fare_sum_type_DF_pv.loc['2019-01-01':'2019-04-29']
     fare_sum_type_DF_pv_loc

     #The "date" index was set to datetime datatype. 
     fare_sum_type_DF_pv_loc.index = pd.to_datetime(fare_sum_type_DF_pv_loc.index)

     #A new DataFrame was created using the "resample()" function by week 'W' and  to get the sum of the fares for each week.
     fare_sum_type_DF_pv_loc_RES = fare_sum_type_DF_pv_loc.resample('W').sum()
     fare_sum_type_DF_pv_loc_RES

     #The object-oriented interface method was used to plot the resample DataFrame.
     from matplotlib import style
     style.use('fivethirtyeight')
     fare_sum_type_DF_pv_loc_RES.plot(figsize=(15,10))
     plt.ylabel("Fare($USD)",fontsize=16, fontweight="bold")
     plt.xlabel('Date',fontsize=16, fontweight="bold")
     plt.title("Total Fare by City Type",fontsize=24, fontweight="bold")
     lgnd = plt.legend(title_fontsize='11', mode="Expanded",
              scatterpoints=1, loc="best", title=r"$\bf{Types}$")
     plt.tight_layout()
     plt.savefig("analysis/fare_summary_byType")
     plt.legend


## Analysis Summary

### Pyber Summary DateFrame

Based on a review of the Pyber Summary DateFrame the urban city type had the highest total fares, however also had the lowest average fare per ride and driver.  The rural city type had the lowest total fares but had the highest of average fare per ride and driver.  It is likely the higher average fare per ride and driver is due to the longer distance driven in rural area. The data set provided did not contain miles driven per ride or driver. An analysis of this data if available may add value to this analysis. 

### Multiple Line Plot

A review of the multiple line plot suggest that the fares increased sharply at the beginning of January for urban and suburban city types.  After mid-January fares for urban city types continued to rise but at a slightly less sharp rate through mid-February. From mid-February through the end of April urban city type fares fluctuated and then leveled off slightly.  Suburban city type fares after mid-January declined slightly through mid-February then increased and decreased sharply towards the end a February. From the beginning of March through the end of April suburban city type fares fluctuated and then increased sharply. Rural city type fares appeared to fluctuate slightly throughout the data period analyzed but remained steady. 

