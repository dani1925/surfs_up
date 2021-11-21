# surfs_up

## Overview

This repository contains a database "hawaii.sqlite" which contains, among other things, the meteorological information of the island of Hawaii.

An investor has requested statistical information on the temperature on the island, which will help him make decisions for his investment on the island.

### Create Engine & Reflect Data Base

With the following section of code, the "connection" with the database is made, which in this specific case is a local file, so an IP address or port was not configured.

```
engine = create_engine("sqlite:///hawaii.sqlite")

# reflect an existing database into a new model
Base = automap_base()

# reflect the tables
Base.prepare(engine, reflect=True)

# Save references to each table
Measurement = Base.classes.measurement
Station = Base.classes.station

# Create our session (link) from Python to the DB
session = Session(engine)

```

### Filtering the Data

Our investor has requested the information only for the month of June and wants to compare it with that of the month of December.
To achieve this, it is necessary to filter the data in the database by date, only for the months of June and December.

The code is shown below.

    results = session.query(Measurement.tobs).filter(func.strftime("%m", Measurement.date) == "06").all()

### Statistical Information

To obtain statistical information of the filtered data, a built-in Pandas function called "describe ()" was used.

    # 5. Calculate and print out the summary statistics for the June temperature DataFrame.
    temps_jun_df.describe()


## Results
The following section shows the analysis of the results obtained.

### Count

Inside de describe function, the count results returns the number of not Nan/Null values, this mean that wea re making the analysis over 1700 data points for June and 1517 data points for Dec which is a very razonable good set of thata for one month. 

| Month | Data Points|
| :---: |  :---: |
| June |  1700 |
| December |  1517 |

### Mean

The mean is the summ of all the data values divided by the number of data points. 
The mean can be considered as the simplest digiltal filter if it is calculated recursively.

| Month | Data Points| Mean|
| :---: |  :---: |  :---: |
| June |  1700 | 74.94
| December |  1517 | 71.04


### Standard Deviation

The standard deviation can be considered as the measure of dispersion of the data, that is, how dispersed the data is from the mean.

| Month | Data Points| Mean| Std
| :---: |  :---: | :---: | :---: |
| June  |  1700  | 74.94 | 3.25
| December |1517 | 71.04 | 3.74

### Min & Max

The simplest fields within the describe () function are Min & Max, which return the highest value and the lowest value in the series.

| Month | Data Points| Mean| Std | Min | Max
| :---: |  :---: | :---: | :---: | :---: |:---: |
| June  |  1700  | 74.94 | 3.25 |64.0 |85.0
| December |1517 | 71.04 | 3.74| 56.0 | 83.0

### Quartiles

Cada cuartil determina la separación entre uno y otro subgrupo, dentro de un conjunto de valores estudiados.

| Month | Data Points| Mean| Std | Min | Max | 25%  |  50%  | 75% |
| :---: |  :---: | :---: | :---: | :---: |:---: |:---:|:---:|:---:|
| June  |  1700  | 74.94 | 3.25 |64.0 |85.0 |  73 |75|77
| December |1517 | 71.04 | 3.74| 56.0 | 83.0 | 69|71 |74

## Summary

If we analyze the number of data points, it seems to be a good data set to perform a statistical analysis, added to a standard deviation of approximately 3 ° C.


The variation in the average temperature between the month of June and the month of December is only 3 ° C, however, the variation in the minimum temperature reached a difference of up to 8 ° C

An interesting fact is that the maximum temperature between the months of June and December is practically identical with a variation of only 2 ° C


It is recommended to do a more robust analysis using the information of the quartiles to identify and manage the possible outliers that could alter any result of the previous analysis.